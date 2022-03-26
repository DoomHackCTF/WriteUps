# Case loss
#####  can you help me brute force the case on this base 64 encoded string in order to help me find the secret message?
##### Ciphertext: t0zqufqtq1rge2jsvxqzzjbyqzfur19iyvmznjrfmvnfm2ftex0

The first thing i did was to divide the cipher text into chunks of four and then get all the possible combinations of upper and lowercase letters of the chunk.
```python
for i in range(0, len(flag_enc), 4):
    temp = flag_enc[i:i+4]
    comb = set(map(''.join, itertools.product(*zip(temp.upper(), temp.lower()))))
```
Then i decoded these chunks and printed the ones that didn't have any weird characters and/or were readable. 
```python
for part in comb:
    try :          
        plain = (base64.b64decode(part.ljust(8, '=')).decode('utf-8'))
      
        if not has_weird_chars(plain):
            print(part, plain)
    except:
        pass
```
And from there i remade the original text with the parts that made sense.
```shell
❯ python solve.py
T0ZQ OFP --
T0Zq OFj
UFQt PT- --
Q1Rg CT`
Q1RG CTF --
e2Js {bl
e2JS {bR --
VXQz Ut3 --
ZjBy f0r --
ZjBY f0X
QzFu C1n --
QzFU C1T
R19I G_H
R19i G_b --
YVMz aS3 --
NjRF 64E
NjRf 64_ --
MVNF 1SE
MVNf 1S_ --
M2Ft 3am
M2FT 3aS --
eX0 y} --
```
**OFPPT-CTF{bRUt3f0rC1nG_baS364_1S_3aSy}**

Full code
```python
import itertools
import base64
import string
def has_weird_chars(text):
    for c in text:
        o = ord(c)
        if o < 33 or o > 126:
            return True
    return False
alphabet = string.ascii_letters + "0123456789"
#print(alphabet)
flag_enc = "t0zqufqtq1rge2jsvxqzzjbyqzfur19iyvmznjrfmvnfm2ftex0"
flag = ""
# every 4 characters of base64 are 3 characters of the original text
for i in range(0, len(flag_enc), 4):
    temp = flag_enc[i:i+4]
    comb = set(map(''.join, itertools.product(*zip(temp.upper(), temp.lower()))))
    for part in comb:
        try :          
            plain = (base64.b64decode(part.ljust(8, '=')).decode('utf-8'))
          
            if not has_weird_chars(plain):
                print(part, plain)
        except:
            pass
```
