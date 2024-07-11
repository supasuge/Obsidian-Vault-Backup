## Table of Contents

    - [Vigenere Solution](#Vigenere\Solution)


```bash
#!/bin/bash
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose
```




### Vigenere Solution
```python
def segment_ciphertext(ciphertext, key_length):
    """Segments the ciphertext into blocks"""
    return [ciphertext[i::key_length] for i in range(key_length)]
print('\n'.join(segment_ciphertext(string, 3)))

```