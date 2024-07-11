## Table of Contents

- [based as 64](#based\as\64)
- [can you xor or not](#can\you\xor\or\not)
- [encoding and boating](#encoding\and\boating)
- [Caesar salad](#caesar\salad)
- [vigenere oh my](#vigenere\oh\my)
- [params and RSA](#params\and\rsa)
- [tricky wien](#tricky\wien)
- [probability ability](#probability\ability)
- [Additional questions](#additional\questions)

# based as 64
*source code not provided*
Decode the string below:
`R3JpenpDVEZ7YmFzZTY0X2lzX3MwX2JhczNkfQ==`

# can you xor or not
*source code not provided*
Ciphertext: `20000000002f2d19191614102c0a0b1a0f18334d001110032d12132d1a0f181b18261f`

Key: `grizzly_bears`

# encoding and boating
*Source code not provided*
I made a really fancy encoder that is totally original can you decode it?
`KIZUU4DFNZYEIVSFLI3WI3KWPFSVMOJRMJWWY6DEK5LGMWSXGVVGEMSSOBRG2ZD2LAZGQMLBIQ4TS===`


# Caesar salad
Can you decrypt the string below? Don't forget about the numbers!

`JulccFWI{4_7op35w_wu4fn6g_b37}`

*Source Code provided:*
```python
NOTCAESAR = lambda char, shift: (
    chr((ord(char) - 65 + shift) % 26 + 65) if char.isupper() else
    chr((ord(char) - 97 + shift) % 26 + 97) if char.islower() else
    str((int(char) + shift) % 10) if char.isdigit() else char)

shift = 3
cipher = ''.join(NOTCAESAR(char, shift) for char in flag)
print('flag: ', cipher)
with open('ciphertext.txt', 'w') as f:
    f.write(cipher)
    f.close()
```


# vigenere oh my
```python
flag = '[REDACTED]'
###############################################################################################
#RESOURCES:
#https://inventwithpython.com/hacking/chapter21.html
#https://www.cipherchallenge.org/wp-content/uploads/2020/12/Five-ways-to-crack-a-Vigenere-cipher.pdf
#https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher
###############################################################################################
import string
uppers = string.ascii_uppercase
def encrypt(pt, key):
	ct = ''
	for i in range(len(pt)):
		p = uppers.index(pt[i])
		k = uppers.index(key[i%len(key)])
		c = (p + k) % 26
		ct += uppers[c]
	return ct
KNOWNPLAINTEXT = 'GRIZZLYBEARCTF ILOVEGRIZZLY BEARSGRIZZLY BEARS AND CTF MAKES ME HAPPY'
KNOWNPLAINTEXT = KNOWNPLAINTEXT.replace(' ', '')
enc = encrypt(flag+KNOWNPLAINTEXT, '[REDACTED]')
print(enc)
with open('ciphertext.txt', 'w') as f:
    f.write("Ciphertext: " + enc + '\n\n')
    f.write("Key: " + "[REDACTED]"+ "\n\n")
    f.write("HINT: please note that '{' and '}' have been removed from the flag.\nThe flag format is all capital letters and begins with GRIZZCTF..............")
```
# params and RSA
Given the parameters from the [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem) algorithm, can you solve for $d$ and decrypt the ciphertext?
`p = 9790033357787902005628417614999315959000084201523262040700298727653371958128646039124393123994245268992455307481075902637781937145592138404400005352767103`
`q = 10651456881313733691372104265794567144976184449167231860425040551030992958899121366932385768695604668001249522119787855706905649381978213089523425109764629`
`e = 65537`
`n = 104278118177100947060399344800502167170643243080994066851130223533195338215223909015083994932511117456450356150276692742542639783979285221007579075230564686483241084168874639846807694189473352800691989056391091955541508019486991863373972388817720407871450295725066067281140870628423215600601843032270184199787`

`ciphertext = 77709769024772952762963763220974515469958924299182866177292877321534506322316405570003271767602285106841440687229681171370846642966819377206789451138643057815597660082214572570435282883650246382279178300762145514842921676568202280400100560377328424229020737341290923102130872396522944839103646998365420044832`

*Source code:*
```python
p = getPrime(512)
q = getPrime(512)
n = p*q
phi = (p-1)*(q-1)
D = inverse(65537, phi)
e = 65537


with open('parameters.txt', 'w') as f:
    f.write(f'p = {p}\nq = {q}\ne = {e}\nn = {n}\n\nciphertext = {pow(bytes_to_long(flag), e, n)}\n\n')

```


# tricky wien
Bob uses RSA to send an encrypted message to Alice...

`The public exponent (e) is 74055340431099766408882267936707642776010581650226201435272792497407293550292032013365925471952817567082880719598406769094568979789928119562145914243280878464502052173375594901651274318376175184252526143746832711593099473243252199500624302661789011938943589291504715025871829888318478230201298680279927185733`

`The public modulus (N) is 97838914831828271794696554308495184238240010208735264833579555635753067640434614260046620044873849882180913747606655702840215394750887109650142301939794552635889224306341701134323183224147183747372000979247618403209966043006121584995372147645237890163928903207087981410772075930324948247238594610177554797321`

`Ciphertext: 16861587669946001223376806426754973317306019873744431685975187231903035170674446121012586442191625287148010783417525587119378816364236506893049174859369649958165190503787679501681754293043786718562966424512787855312521596063364606668169962695198065691250458367098623875931167644186298697657888093564424546097`
What was the important message being sent? 

*Source code provided:*
```python
from Crypto.Util.number import long_to_bytes,bytes_to_long
import Crypto
import json
from math import isqrt

from sympy import *
import random
def get_prime(bits):
	p = Crypto.Util.number.getPrime(bits, randfunc=Crypto.Random.get_random_bytes)
	return(p)

def create_keypair(size):
    while True:
        p = get_prime(size // 2)
        q = get_prime(size // 2)
        if q < p < 2*q:
            break

    N = p * q
    phi_N = (p - 1) * (q - 1)

    max_d = isqrt(isqrt(N))// 3

    max_d_bits = max_d.bit_length() - 1

    while True:
        d = random.randint(0,2**max_d_bits)
        try:
            e = pow(d,-1,phi_N)
        except: 
            continue
        if (e * d) % phi_N == 1:
            break

    return  N, e, d, p, q

def challenge():
    with open('/home/yaditydo/Desktop/CTFs/OU_OFFICIAL_CTF/.flags.json', 'r') as m:
        data=json.load(m)
        flag = data['crypto']['flag7'].encode('utf-8')
    
    bits=1024


    m=  bytes_to_long(flag)

    N, e, d, p, q = create_keypair(bits)

    C=pow(m,e,N)
    with open("challenge.txt", "w") as f:
        f.write(f'Bob uses RSA to send an encrypted message to Alice...\n\nThe public exponent (e) is {e}\n\nThe public modulus (N) is {N}\n\nCiphertext: {C}\r\nCan you decrypt the ciphertext using Wiener\'s attack?')
        f.close()

if __name__ == "__main__":
    challenge()
```





# probability ability
```
            # Consider the following five events:
------------------------------------------------------------------------------------            
- Correctly guessing a random 128-bit AES key on the first try.
- Winning a lottery with 1 million contestants (the probability is 1/106 ).
- Winning a lottery with 1 million contestants 5 times in a row (the probability is (1/106)5 ).
- Winning a lottery with 1 million contestants 6 times in a row.
- Winning a lottery with 1 million contestants 7 times in a row.

What is the order of these events from most likely to least likely?

Answer format: int, int, int, int, int

Note: Once you have your answer, simply connect to the netcat service and submit your answer.
x, x, x, x, x

The correct answer will return the flag!

Good luck!
```

This is a challenge with a running service to `nc` into.

```python
import socketserver
import threading

# Additional questions
QUESTIONS = {
    "What is the command to list all files in a directory?": ["A. ls", "B. dir", "C. cd", "D. tree", "A"],
    "What is the command to change directory?": ["A. ls", "B. dir", "C. cd", "D. tree", "C"],
    "What is the command to list all running auxiliary processes?": ["A. ps aux", "B. kill", "C. top", "D. killall", "A"],
    "What is the command to change to the root user of the system?": ["A. sudo", "B. su -", "C. root", "D. sudo su", "B"],
    "What is the correct path in which DNS Name resolutions are stored?": ["A. /etc/hosts", "B. /etc/resolv.conf", "C. /etc/hostname", "D. /etc/hosts.conf", "B"],
    "What is the command to show the current working directory?": ["A. pwd", "B. cd", "C. ls", "D. dir", "A"],
    "What command is used to display the first lines of a file?": ["A. tail", "B. less", "C. head", "D. cat", "C"],
    "What is the command to find files in a directory hierarchy?": ["A. grep", "B. find", "C. locate", "D. ls", "B"],
    "What is the command to display disk usage?": ["A. df", "B. du", "C. diskfree", "D. diskusage", "A"],
    "What command shows the manual page of a command?": ["A. man", "B. help", "C. info", "D. cmd", "A"]
}

FLAG = "GrizzCTF{n0w_y04_kn0w_4_l1l_l1nux_3h}"

class QuizHandler(socketserver.BaseRequestHandler):
    def handle(self):
        self.request.sendall("Welcome to the Linux CLI Quiz!\n".encode())
        correct_answers = 0
        for question, choices in QUESTIONS.items():
            self.request.sendall(f"\n{question}\n".encode())
            for choice in choices[:-1]:
                self.request.sendall(f"{choice}\n".encode())
            self.request.sendall("Your answer (A, B, C, D): ".encode())
            answer = self.request.recv(1024).strip().upper()
            if answer == choices[-1].encode():
                correct_answers += 1
                self.request.sendall("Correct!\n".encode())
            else:
                self.request.sendall("Incorrect. Try again next time!\n".encode())
                return
        if correct_answers == len(QUESTIONS):
            self.request.sendall(f"Congratulations! Here's your flag: {FLAG}\n".encode())

class ThreadedQuizServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
    allow_reuse_address = True

if __name__ == "__main__":
    HOST, PORT = "localhost", 1234

    with ThreadedQuizServer((HOST, PORT), QuizHandler) as server:
        server_thread = threading.Thread(target=server.serve_forever)
        server_thread.start()
        print(f"Server started at {HOST}:{PORT}")

        try:
            server_thread.join()
        except KeyboardInterrupt:
            server.shutdown()
            server.server_close()
            print("Server stopped.")
```

