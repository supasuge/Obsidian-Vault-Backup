## Table of Contents

      - [MSF - Directory Structure](#MSF\-\Directory\Structure)
      - [MSF - Loading Additional Modules at Runtime](#MSF\-\Loading\Additional\Modules\at\Runtime)
      - [MSF - Loading Additional Modules](#MSF\-\Loading\Additional\Modules)
  - [Porting Over Scripts into Metasploit Modules](#Porting\Over\Scripts\into\Metasploit\Modules)
      - [Porting MSF Modules](#Porting\MSF\Modules)

The default directory where all the modules, scripts, plugins, and `msfconsole` proprietary files are stored is `/usr/share/metasploit-framework`. The critical folders are also symlinked in our home and root folders in the hidden `~/.msf4/` location.

#### MSF - Directory Structure

Writing and Importing Modules

```shell
gdxqpardo@htb[/htb]$ ls /usr/share/metasploit-framework/

app     db             Gemfile.lock                  modules     msfdb            msfrpcd    msf-ws.ru  ruby             script-recon  vendor
config  documentation  lib                           msfconsole  msf-json-rpc.ru  msfupdate  plugins    script-exploit   scripts
data    Gemfile        metasploit-framework.gemspec  msfd        msfrpc           msfvenom   Rakefile   script-password  tools
```

Writing and Importing Modules

```shell-session
gdxqpardo@htb[/htb]$ ls .msf4/

history  local  logos  logs  loot  modules  plugins  store
```

We copy it into the appropriate directory after downloading the [exploit](https://www.exploit-db.com/exploits/9861). Note that our home folder `.msf4` location might not have all the folder structure that the `/usr/share/metasploit-framework/` one might have. So, we will just need to `mkdir` the appropriate folders so that the structure is the same as the original folder so that `msfconsole` can find the new modules. After that, we will be proceeding with copying the `.rb` script directly into the primary location.

Please note that there are certain naming conventions that, if not adequately respected, will generate errors when trying to get `msfconsole` to recognize the new module we installed. Always use snake-case, alphanumeric characters, and underscores instead of dashes.

For example:

- `nagios3_command_injection.rb`
- `our_module_here.rb`

#### MSF - Loading Additional Modules at Runtime

```shell
gdxqpardo@htb[/htb]$ cp ~/Downloads/9861.rb /usr/share/metasploit-framework/modules/exploits/unix/webapp/nagios3_command_injection.rb
gdxqpardo@htb[/htb]$ msfconsole -m /usr/share/metasploit-framework/modules/
```

#### MSF - Loading Additional Modules

```shell
msf6> loadpath /usr/share/metasploit-framework/modules/
```


lternatively, we can also launch `msfconsole` and run the `reload_all` command for the newly installed module to appear in the list. After the command is run and no errors are reported, try either the `search [name]` function inside `msfconsole` or directly with the `use [module-path]` to jump straight into the newly installed module.

```shell
msf6 > reload_all
msf6 > use exploit/unix/webapp/nagios3_command_injection 
msf6 exploit(unix/webapp/nagios3_command_injection) > show options

Module options (exploit/unix/webapp/nagios3_command_injection):

   Name     Current Setting                 Required  Description
   ----     ---------------                 --------  -----------
   PASS     guest                           yes       The password to authenticate with
   Proxies                                  no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                                   yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    80                              yes       The target port (TCP)
   SSL      false                           no        Negotiate SSL/TLS for outgoing connections
   URI      /nagios3/cgi-bin/statuswml.cgi  yes       The full URI path to statuswml.cgi
   USER     guest                           yes       The username to authenticate with
   VHOST                                    no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target
```


## Porting Over Scripts into Metasploit Modules

To adapt a custom Python, PHP, or any type of exploit script to a Ruby module for Metasploit, we will need to learn the Ruby programming language. Note that Ruby modules for Metasploit are always written using hard tabs.

When starting with a port-over project, we do not need to start coding from scratch. Instead, we can take one of the existing exploit modules from the category our project fits in and repurpose it for our current port-over script. Keep in mind to always keep our custom modules organized so that we and other penetration testers can benefit from a clean, organized environment when searching for custom modules.

We start by picking some exploit code to port over to Metasploit. In this example, we will go for [Bludit 3.9.2 - Authentication Bruteforce Mitigation Bypass](https://www.exploit-db.com/exploits/48746). We will need to download the script, `48746.rb` and proceed to copy it into the `/usr/share/metasploit-framework/modules/exploits/linux/http/` folder. If we boot into `msfconsole` right now, we will only be able to find a single `Bludit CMS` exploit in the same folder as above, confirming that our exploit has not been ported over yet. It is good news that there is already a Bludit exploit in that folder because we will use it as boilerplate code for our new exploit.

#### Porting MSF Modules

Writing and Importing Modules

```shell
gdxqpardo@htb[/htb]$ ls /usr/share/metasploit-framework/modules/exploits/linux/http/ | grep bludit

bludit_upload_images_exec.rb
```

Writing and Importing Modules

```shell
gdxqpardo@htb[/htb]$ cp ~/Downloads/48746.rb /usr/share/metasploit-framework/modules/exploits/linux/http/bludit_auth_bruteforce_mitigation_bypass.rb
```





