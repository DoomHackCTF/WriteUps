# XOR 
#####  Some workers convinced me to invest in their cryptosystem. They said their brand new XOR keystream cipher would revolutionize the crypto world.
##### Luckily they dropped a piece of paper with the source code on it and an encrypted flag. Can you prove they are wrong?!

We have the following ciphertext:
```
48ac3fe745bed053ac14ef2163cc70db58dfe862fe33860330dc22ea589c9f03d922e13365db038626eaee
```
And the following code:
```python
import os

FLAG = b'OFPPT-CTF{...REDACTED...}'


class HoneyComb:
    def __init__(self, key):
        self.vals = [i for i in key]

    def turn(self):
        self.vals = [self.vals[-1]] + self.vals[:-1]

    def encrypt(self, msg):
        keystream = []
        while len(keystream) < len(msg):
            keystream += self.vals
            self.turn()
        return bytes([msg[i] ^ keystream[i] for i in range(len(msg))]).hex()

hc = HoneyComb(os.urandom(6))

print(hc.encrypt(FLAG))
```
The key is only 6 bytes long, to match the length of the plaintext it is reused moving the last position to the first in each iteration.

Since we know the start of the plaintext (OFPPT-) finding the key isn't hard. 
```python
def find_first_six(enc):
    flag_vals = []
    for i in range (0, 12, 2):
        temp = int(enc[i:i+2],16)
        for j in range (255):
            if i == 0 and j ^ temp == 79:
                flag_vals.append(j)
                break
            if i == 2 and j ^ temp == 70:
                flag_vals.append(j)
                break
            if i == 4 and j ^ temp == 80:
                flag_vals.append(j)
                break
            if i == 6 and j ^ temp == 80:
                flag_vals.append(j)
                break
            if i == 8 and j ^ temp == 84:
                flag_vals.append(j)
                break
            if i == 10 and j ^ temp == 45:
                flag_vals.append(j)
                break
    return (flag_vals)
```
Know we do the same process as the encryption to decrypt the cipher.
```python
def decrypt(enc, vals):
    keystream = []
    while len(keystream) < len(enc):
        keystream += vals
        vals = turn(vals)
    return "".join(chr(enc[i] ^ keystream[i]) for i in range(len(enc)))
```

Full code:
```python
def find_first_six(enc):
    flag_vals = []
    for i in range (0, 12, 2):
        temp = int(enc[i:i+2],16)
        for j in range (255):
            if i == 0 and j ^ temp == 79:
                flag_vals.append(j)
                break
            if i == 2 and j ^ temp == 70:
                flag_vals.append(j)
                break
            if i == 4 and j ^ temp == 80:
                flag_vals.append(j)
                break
            if i == 6 and j ^ temp == 80:
                flag_vals.append(j)
                break
            if i == 8 and j ^ temp == 84:
                flag_vals.append(j)
                break
            if i == 10 and j ^ temp == 45:
                flag_vals.append(j)
                break
    return (flag_vals)
 
def to_decimal(enc):
    dec = []
    for i in range(0,len(enc),2):
        temp = int(enc[i:i+2],16)
        dec.append(temp)
    return dec

def turn(vals):
    vals = [vals[-1]] + vals[:-1]
    return vals

def decrypt(enc, vals):
    keystream = []
    while len(keystream) < len(enc):
        keystream += vals
        vals = turn(vals)
    return "".join(chr(enc[i] ^ keystream[i]) for i in range(len(enc)))


enc = "48ac3fe745bed053ac14ef2163cc70db58dfe862fe33860330dc22ea589c9f03d922e13365db038626eaee"
vals = find_first_six(enc)
dec = to_decimal(enc)
print(decrypt(dec, vals))
```
```shell
❯ python solve.py
OFPPT-CTF{X0r_w17h_sm4ll_k3y_vuln3r4b1l17y}
```
