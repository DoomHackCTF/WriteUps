<img src="img/challenge.png">

This is the cipher.

<img src="img/3fa7aeee79c660ea86111702f0d4953f.png">

We have a hint of the cipher in the description of the challenge (**pig-pen**), i searched for a decoder online and [found one](https://planetcalc.com/7842/), i introduced the flag and got the following result.


<img src="img/descifrado.png">

I just needed to convert it to lowercase.

```python
>>> "GOODGRIEFCHARLIEBROWN".lower()
'goodgriefcharliebrown'
```

**The flag is: flag{goodgriefcharliebrown}**
