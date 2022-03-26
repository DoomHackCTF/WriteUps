# Meet Me In Middle 
#####  A company has deployed their own cryptographic service. They challenged anyone to break it even if they have shared the source code. The keys are unknown to everyone but them.
##### Fortunately, their encryption algorithm is vulnerable. Could you help us break the encryption and read their secret flag? 

We have the following encrypted text:
```
Super strong encryption service approved by 2022 stansdards.
Message to decrypt:
187f25ea856f518bcd8e7e7c17e7e6016bc77459513740e6792c84d07b465ea9cee6609881421eb4ae1606792a2d8859
Encrypt your text:
>
{"pt":"4f465050542d435446"}
f71f3b195e2336a6d30077b8184304c6
```
And the following code:
```python
from random import randint
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import json

flag = b'OFPPT-CTF{Not_the_real_flag}'

def gen_key(option=0):
    alphabet = b'0123456789abcdef'
    const = b'0fpptCTF5!@#'
    key = b''
    for i in range(16-len(const)):
        key += bytes([alphabet[randint(0,15)]])

    if option:
        return key + const
    else:
        return const + key

def encrypt(data, key1, key2):
    cipher = AES.new(key1, mode=AES.MODE_ECB)
    ct = cipher.encrypt(pad(data, 16))
    cipher = AES.new(key2, mode=AES.MODE_ECB)
    ct = cipher.encrypt(ct)
    return ct.hex()


def challenge():
    k1 = gen_key()
    k2 = gen_key(1)

    
    ct = encrypt(flag, k1, k2)
    
    
    print('Super strong encryption service approved by 2022 stansdards.\n'+\
                    'Message to decrypt:\n' +ct + '\nEncrypt your text:\n> ')
    try:
            
        dt = json.loads(input().strip())
        pt = bytes.fromhex(dt['pt'])
        res = encrypt(pt, k1, k2)
        print(res + '\n')
        exit(1)
    except Exception as e:
        print(e)
        print('Invalid payload.\n')
        exit(1)
    
if __name__ == "__main__":
    challenge()
```
As we can see, the flag is encrypted twice with **AES-ECB** with two different keys, and after that the program asks us to input a plaintext and then ecrypts it with the same two keys.

The key generation is weak due to it having only 4 random characters and we know the alphabet

This is a **Meet in the middle attack** and the problem is almost the exact same as the one in this [writeup](https://int80h.netlify.app/writeup/byte-bandits-2020-meet-me-there/).

What we need to do the is the following:
* Encrypt the plain text we are given with all the possible key1 and save all the ciphertexts with their respective keys.
* Decrypt the ciphertext we are given with all the possible key2.
* If we find a coincidence it means we found both keys.

(Code taken from the writeup mentioned above).

```python
#!/usr/bin/python

import binascii
import sys
import random
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
from string import printable

pt = '4f465050542d435446'
ct = 'f71f3b195e2336a6d30077b8184304c6'
encrypted_flag = '187f25ea856f518bcd8e7e7c17e7e6016bc77459513740e6792c84d07b465ea9cee6609881421eb4ae1606792a2d8859'

ciphertext_dict = {}
alphabet = '0123456789abcdef'
const = '0fpptCTF5!@#'
for i in alphabet:
    for j in alphabet:
        for k in alphabet:
            for l in alphabet:
                suffix = ''.join([i, j, k, l])
                key1 = const + suffix
                cipher1 = AES.new(key=bytes(key1,'utf-8'), mode=AES.MODE_ECB)
                c1 = cipher1.encrypt(pad(bytes.fromhex(pt),16))
                ciphertext_dict[c1] = suffix

win_suffix = ""
win_prefix = ""

for i in alphabet:
    for j in alphabet:
        for k in alphabet:
            for l in alphabet:
                prefix = ''.join([i, j, k,l])
                key2 = prefix + const
                cipher2 = AES.new(key=bytes(key2, 'utf-8'), mode=AES.MODE_ECB)
                c2 = cipher2.decrypt(bytes.fromhex(ct))
                if c2 in ciphertext_dict:
                    print("[+] Wow, found {} {}".format(binascii.hexlify(bytes(ciphertext_dict[c2], 'utf-8')), binascii.hexlify(bytes(prefix,'utf-8'))))
                    win_suffix = ciphertext_dict[c2]
                    win_prefix = prefix

key1 = const + win_suffix
key2 = win_prefix + const 
print(key1)
print(key2)
c = AES.new(key=bytes(key2, 'utf-8'), mode=AES.MODE_ECB)
middle = c.decrypt(bytes.fromhex(encrypted_flag))
c = AES.new(key=bytes(key1,'utf-8'), mode=AES.MODE_ECB)
flag = binascii.unhexlify(c.decrypt(middle).hex())
print(flag)
```
```shell
❯ python solve.py
[+] Wow, found b'32663734' b'65613134'
0fpptCTF5!@#2f74
ea140fpptCTF5!@#
b'OFPPT-CTF{M33t_1n_Th3_Middle_4tt4ck_4_RS4}\x06\x06\x06\x06\x06\x06'
```
