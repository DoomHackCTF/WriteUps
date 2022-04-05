<img src="img/challenge.png">

We are given a .tar that contains two .txt files, one containing users and the other the passwords.

Using grep i found the line where the user is.
```shell
❯ grep -ni cultiris usernames.txt
378:cultiris
```
Then i printed the password in that line using awk.
```shell
❯ awk '{if(NR==378) print $0}' passwords.txt
cvpbPGS{P7e1S_54I35_71Z3}
```
It looks like rot13, so i deciphered it.
```shell
❯ awk '{if(NR==378) print $0}' passwords.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
picoCTF{C7r1F_54V35_71M3}
```
