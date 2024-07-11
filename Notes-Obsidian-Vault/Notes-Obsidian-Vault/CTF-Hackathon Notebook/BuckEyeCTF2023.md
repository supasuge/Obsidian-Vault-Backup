## Table of Contents

- [Buckeye2023](#buckeye2023)
- [aaaa aaab ctf](#aaaa\aaab\ctf)
- [aaaa aabc tf{](#aaaa\aabc\tf{)

# Buckeye2023

Source Code: 
```Python
from Crypto.Cipher import AES
from flask import Flask, request, abort, send_file
import math
import os

app = Flask(__name__)

key = os.urandom(32)
flag = os.environ.get('FLAG', 'bctf{fake_flag_fake_flag_fake_flag_fake_flag}')

cipher = AES.new(key, AES.MODE_ECB)

def encrypt(message: str) -> bytes:
    length = math.ceil(len(message) / 16) * 16
    padded = message.encode().ljust(length, b'\0')
    return cipher.encrypt(padded)

@app.get('/encrypt')
def handle_encrypt():
    param = request.args.get('message')

    if not param:
        return abort(400, "Bad")
    if not isinstance(param, str):
        return abort(400, "Bad")

    return encrypt(param + flag).hex()

@app.get('/source')
def handle_source():
    return send_file(__file__, "text/plain")

@app.get('/')
def handle_home():
    return """
        <style>
            form {
                display: flex;
                flex-direction: column;
                max-width: 20em;
                gap: .5em;
            }

            input {
                padding: .4em;
            }
        </style>
        <form action="/encrypt">
            <h2><i>ELECTRONICAL</i></h2>
            <label for="message">Message to encrypt:</label>
            <input id="message" name="message"></label>
            <input type="submit" value="Submit">
            <a href="/source">Source code</a>
        </form>
    """

if __name__ == "__main__":
    app.run()

```

AES-ECB? Ez Clap.

![Hecker](https://imgs.search.brave.com/FoaeHobQWyN68okqhFXoQ8suqw5TwQ77lKD7FvvGwIg/rs:fit:500:0:0/g:ce/aHR0cHM6Ly9pLmlt/Z2ZsaXAuY29tLzQv/MXR4NGcuanBn "a title")

```Python
import requests

url = lambda x: f'https://electronical.chall.pwnoh.io/encrypt?message={x}'

import string
chars = string.printable

#
#
# aaaa aaab ctf
# aaaa aabc tf{
#

def generate(prefix):
    return [prefix + c for c in chars]

from urllib import parse
def get_enc(x):
    print([x[i:i+16] for i in range(0, len(x), 16)])
    return requests.get(url(parse.quote(x))).text

known = 'a' * 16

for block_idx in range(5):
    for i in range(16):
        strings = generate(known[-15:])
        res = get_enc(''.join(strings) + 'a' * (15-i))
        blocks = [res[i:i+32] for i in range(0, len(res), 32)]
        print(blocks[0:len(strings)])
        print(blocks[len(chars)+block_idx])
        idx = [i for i in range(len(chars)) if blocks[i] == blocks[len(chars) + block_idx]][0]
        known += chars[idx]
        print("Known: {}\n".format(known))

print("Known: {}\n".format(known))
```
I am an absolute filthy animal for this. I know....
