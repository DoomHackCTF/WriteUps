<img src="img/challenge.png">

I decoded the text in this [page](https://onlinestringtools.com/convert-ascii-to-string), it was octal ascii, this gives us a hexadecimal string.

<img src="img/1.png">

After decoding the hexadecimal we have a base64.

<img src="img/2.png">

Decode the base64.

```shell
echo "ZmxhZ3ttbW1fcEAkdGF9" | base64 -d
flag{mmm_p@$ta}
```
