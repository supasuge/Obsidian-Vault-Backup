## Table of Contents

    - [LD.SO: The dynamic linker/loader](#LD.SO:\The\dynamic\linker/loader)
  - [Exploitation](#Exploitation)

### LD.SO: The dynamic linker/loader
Whenever u execute an ELF file that depends on shared libs (.so files), the OS will need to load such libs and link them to ur executable so that any shared functions are available to your program during its execution. In linux, normally managed by `ld.so` , an exe prepackaged as part of the `glib` library.

Every ELF executable contains a section named `.interp`, where the location of its required loader is specified so that the operating system knows which specific loader to use. For most ELF files in Linux, this will be `ld.so` (probably with a slightly different name). To check the `.interp` section of an ELF file, you can use the following command:

Terminal

```shell
user@attackbox$ readelf /usr/bin/man -p .interp

String dump of section '.interp':
  [     0]  /lib64/ld-linux-x86-64.so.2
```


We can also check which libraries are needed to run the `man` command by using the `ldd` command to get a list of dependencies:

```bash
user@attackbox$ ldd /usr/bin/man 
linux-vdso.so.1 (0x00007ffc699f7000)
libmandb-2.11.2.so => /usr/lib/man-db/libmandb-2.11.2.so (0x00007f24dc8f4000)
libman-2.11.2.so => /usr/lib/man-db/libman-2.11.2.so (0x00007f24dc8c2000)
libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007f24dc886000)
libpipeline.so.1 => /lib/x86_64-linux-gnu/libpipeline.so.1 (0x00007f24dc874000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f24dc692000)
libgdbm.so.6 => /lib/x86_64-linux-gnu/libgdbm.so.6 
(0x00007f24dc67d000)
libseccomp.so.2 => /lib/x86_64-linux-gnu/libseccomp.so.2 (0x00007f24dc65d000)
/lib64/ld-linux-x86-64.so.2 (0x00007f24dca1d000)```


**Using DT_RPATH to Influence the Library Search Path**
When compiling a program, you can specify additional paths where you want `ld.so` to look for libraries. These directories will be embedded in your program's ELF file and parsed and used by ld.so when loading the executable. This allows you to override the default library search path and have your program search in an alternate location first. For example, you could compile a program with the following options to that end:

```bash
user@attackbox$ gcc -Wl,--enable-new-dtags -Wl,-rpath=/tmp -o myapp myapp.c
```

**Modifying glibc Behaviour via GLIBC_TUNABLES**

To better understand how this vulnerability works, let's have a look at how the dynamic linker (ld.so) briefly works. Once it gets executed, it checks specific environment variables called GLIBC_TUNABLES. Those features, aka Tunables, allow developers to dynamically alter the runtime library behaviour.

One example to understand how this process works is the following:

```c
GLIBC_TUNABLES="malloc.check=1:malloc.tcache_max=128"
```

The environment variable `GLIBC_TUNABLES` sets the maximum size chunk that may be stored in a `tcache` (in bytes).

In line 282, the code looks for any variables with the name `GLIBC_TUNABLES` in the environment variables (`envp`) and then copies them into `new_env`. Next, the method `parse_tunables()`, called (at line 286), processes and sanitizes the data in `new_env` to ensure that the settings in `GLIBC_TUNABLES` won't result in any problems or vulnerabilities.

The processed copy `new_env` is substituted for the original `GLIBC_TUNABLES` variable at line 288.
```C
```c
269 void
270 __tunables_init (char **envp)
271 {
272   char *envname = NULL;
273   char *envval = NULL;
274   size_t len = 0;
275   char **prev_envp = envp;
...
279   while ((envp = get_next_env (envp, &envname, &len, &envval,
280                                &prev_envp)) != NULL)
281     {
282       if (tunable_is_name ("GLIBC_TUNABLES", envname))
283         {
284           char *new_env = tunables_strdup (envname);
285           if (new_env != NULL)
286             parse_tunables (new_env + len + 1, envval);
287           /* Put in the updated envval.  */
288           *prev_envp = new_env;
289           continue;
290         }
```
Inside `parse_tunables()`, the code examines the copied `GLIBC_TUNABLES` to break it down into individual name-value pairs. It does this by searching for equal signs (=) and colons (:) in the copied data.

The vulnerability occurs when `GLIBC_TUNABLES` contains unexpected input, like `tunable1=tunable2=AAA`. In this case, instead of gracefully handling it, the code copies the entire input as if it were a valid setting.  This issue occurs when the tunables are of type `SXID_IGNORE`, which should not be removed. During the first iteration of the loop, the entire `tunable1=tunable2=AAA` is copied to tunestr, filling it up. Later, at lines 247-248, the code fails to increment the pointer (p) because no colon (':') was found. As a result, p still points to the value of `tunable1`, i.e., `tunable2=AAA`. During the second iteration of the loop, `tunable2=AAA` is incorrectly appended to `tunestr`, causing a buffer overflow because `tunestr` is already full.

```c
162 static void
163 parse_tunables (char *tunestr, char *valstring)
164 {
...
168   char *p = tunestr;
169   size_t off = 0;
170 
171   while (true)
172     {
173       char *name = p;
174       size_t len = 0;
175 
176       /* First, find where the name ends.  */
177       while (p[len] != '=' && p[len] != ':' && p[len] != '\0')
178         len++;
179 
180       /* If we reach the end of the string before getting a valid name-value
181          pair, bail out.  */
182       if (p[len] == '\0')
183         {
184           if (__libc_enable_secure)
185             tunestr[off] = '\0';
186           return;
187         }
188 
189       /* We did not find a valid name-value pair before encountering the
190          colon.  */
191       if (p[len]== ':')
192         {
193           p += len + 1;
194           continue;
195         }
196 
197       p += len + 1;
198 
199       /* Take the value from the valstring since we need to NULL terminate it.  */
200       char *value = &valstring[p - tunestr];
201       len = 0;
202 
203       while (p[len] != ':' && p[len] != '\0')
204         len++;
205 
206       /* Add the tunable if it exists.  */
207       for (size_t i = 0; i < sizeof (tunable_list) / sizeof (tunable_t); i++)
208         {
209           tunable_t *cur = &tunable_list[i];
210 
211           if (tunable_is_name (cur->name, name))
212             {
...
219               if (__libc_enable_secure)
220                 {
221                   if (cur->security_level != TUNABLE_SECLEVEL_SXID_ERASE)
222                     {
223                       if (off > 0)
224                         tunestr[off++] = ':';
225 
226                       const char *n = cur->name;
227 
228                       while (*n != '\0')
229                         tunestr[off++] = *n++;
230 
231                       tunestr[off++] = '=';
232 
233                       for (size_t j = 0; j < len; j++)
234                         tunestr[off++] = value[j];
235                     }
236 
237                   if (cur->security_level != TUNABLE_SECLEVEL_NONE)
238                     break;
239                 }
240 
241               value[len] = '\0';
242               tunable_initialize (cur, value);
243               break;
244             }
245         }
246 
247       if (p[len] != '\0')
248         p += len + 1;
249     }
250 }
```

## Exploitation
**1. Environment Variables setup**
- exploit creates different array's (line 41) which will be used later to store the `GLIBC_TUNABLES` environment variables and trigger the buffer overflow in glibc when the program is executed.
**2. Environment Variables Crafting**  
  
- `filler`: This variable is created to pad away the loader's read-write section. It's filled with a long sequence of 'F' characters ('F'). (lines 17-23) 
- `kv`: This variable is the payload that will trigger the buffer overflow. It's filled with a long sequence of 'A' characters ('A'). (lines 27-33)  
- `filler2`: Similar to filler, this variable is used to pad away any extra portions. It's also filled with a sequence of 'F' characters. (lines 37-43)  
- `dt_rpath`: This variable is used to craft a specific value `-0x14ULL` to overwrite memory regions in the exploitation process. (lines 47-53)

  
All those variables will be copied to the `envp` array, which contains environment variables.

1. **Weaponization  

    The payload in `GLIBC_TUNABLES` does the following to ld.so: It overwrites a segment of the stack with an index pointing to a string with `"` as its content. The intention here is to manipulate the stack so that the index points to a specific memory address where the character " (a double quote) is stored. The stack is a region of memory used for storing local variables and function call information during program execution. To add to the exploit's complexity, modern operating systems randomize the stack's location, making it difficult to predict where the index will end up in memory.


**Exploitation**:  
- The exploit uses a trial-and-error approach to account for the randomness of the stack's location. It repeatedly runs the program (forking and executing it) until it gets lucky and the fixed address `0x7ffdfffff030` matches the address where the stack index points to ". 
- This effectively manipulates the library search path to point to the directory named `"` which will contain a malicious version of `lib6.so`.
- The process for generating the forged `libc6.so` consists of copying libc6.so but replacing the `__libc_start_main` function with a custom shellcode that does `setuid(0) + setgid(0) + exec('/bin/sh')`. You can find the Python script for this [here](https://github.com/leesh3288/CVE-2023-4911/blob/main/gen_libc.py).
- The program enters a loop where it forks a child process: In the child process, it attempts to execute `/usr/bin/su` with `--help`. If the child process takes longer than one second, it assumes that it is returning from a shell, indicating a potential successful exploit.