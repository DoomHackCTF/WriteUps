<img src="img/challenge.png">

We are given a pdf, but we can't open it, a quick inspection with the file command reveals its a shell file.

After changing the extension of the file we get a lengthy shell script, but this is just a distraction, we have the interesting part at the ending of the script.
```shell
${echo} "x - extracting flag (text)"
  sed 's/^X//' << 'SHAR_EOF' | uudecode &&
begin 600 flag
M(3QA<F-H/@IF;&%G+R`@("`@("`@("`@,"`@("`@("`@("`@,"`@("`@,"`@
M("`@-C0T("`@("`Q,#(T("`@("`@8`K'<6D`&+RD@0`````!````,&(_-P4`
M``#\`69L86<``$)::#DQ05DF4UG'(\VD```=?___9]]9F_E#_OO_V?OYM_)_
M^]E_-*N=Z_XW__?_F_M=[[`!&QB.@`&@`#0!H`-`R``T`>H:``T-```T```&
M0-`&U!DT,FFTC3U'J>33U3U$`!M0T-`/4`````&@!HS4TT&@--`&@!H-J`VD
M`\H>@C3:F@9/4```:-!Z@JI^J`#(::`T:-!HT`T`::`T`:!D#"`T&@#)B!D#
M0&(:!@@!IB&1H`&@`@"``:QW<`%0^<#-`H'%O"!X""9>>CLAD.(^[PL6/A-B
ME6;]I0QU'(^*<M3A4G020JTX$V:S)<UBPJJ^GA05BX1&=/GP?0Q[D'+!CD+P
MV_K+I4K!/7/[]-KD0PR"2X$3<7S15]YM6_(0H3E!5-0!0@?S%M7+4@%KW"`W
M9TXF\AOG*PC:URG`MOO+*RJ="HSSJ-6/DB)$)!9J4C@F+*0#WWI-'H!8/V/.
MN"&.IB25D82>88[8^:1U<C>D_#Z*'38P5K.XK"R0U$BFCIM2#VV_(5N+2"$,
M^0E%Y%8Z1>T,:6*-JLZ>'\T^3?74((H3*_@S"H1E6[0DKAA*00(S#W`"JC<*
M6:^6M[N<>27G/R`1BRH($\$2[("$7&2->AHJ\X`#>0P,$L`J5,#,*/%1BD1!
MO!RC4\T6>?$ZM`$K_%W)%.%"0QR/-I#'<0`````````````!``````````L`
M`````%1204E,15(A(2$`````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
M````````````````````````````````````````````````````````````
,````````````````
`
end
```
We can see that is decoding this thing with uudecode, so this is a binary that was transformed to ascii text using [uuencode](https://linux.die.net/man/1/uuencode). I decoded it manually because executing the script failed, so i copied the text and decoded it using uudecode.
```shell
❯ uudecode flag_enc
```
This gives us the following file.
```shell
❯ cat flag
!<arch>
flag/           0           0     0     644     1024      `
�4ښOPh�~�4h��i4dY&SY�ͤg�C���4�7]�4@�4�
                 4Ɉ@!wpP�ż &^z;!��>bf
                                     ur�RtB8f%�ªFt�
                                                   {rB�˥J=s��
                                                             Kq|��[�9AT�B��Rk�7gN&��)�*
�"D$jR8&,�MX?cθ!$a�ur7>60V,�Rm![H!
                                  	E�:E�ibΞ�M�+3
e[$JA3p7
Yy%� 쀄\dz*�y

             *T��DAS�y�+]��C6�
                              TRAILER!!!⏎                                                                                                           
```
We can see that it has **!\<arch\>** at the beginning, this indicates that it's a [.ar](https://linux.die.net/man/1/ar) file, i changed the extension to .ar and extracted the contents. From this point we have to extract different types of compressed files.
```shell
picoCTF2022/Forensics/File types
❯ ar -x flag.ar

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: cpio archive

picoCTF2022/Forensics/File types
❯ mv flag flag.cpio

picoCTF2022/Forensics/File types
❯ cpio -idv < flag.cpio
flag
2 blocks

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   flag.cpio   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: bzip2 compressed data, block size = 900k

picoCTF2022/Forensics/File types
❯ mv flag flag.bz2

picoCTF2022/Forensics/File types
❯ bzip2 -d flag.bz2

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   flag.cpio   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: gzip compressed data, was "flag", last modified: Tue Mar 15 06:50:39 2022, from Unix, original size modulo 2^32 328

picoCTF2022/Forensics/File types
❯ mv flag flag.gz

picoCTF2022/Forensics/File types
❯ gzip -d flag.gz

picoCTF2022/Forensics/File types
❯ file flag
flag: lzip compressed data, version: 1

picoCTF2022/Forensics/File types
❯ mv flag flag.lzip

picoCTF2022/Forensics/File types
❯ lzip -d flag.lzip

picoCTF2022/Forensics/File types
❯ ls
 flag.ar   flag.cpio   flag.lzip.out   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag.lzip.out
flag.lzip.out: LZ4 compressed data (v1.4+)

picoCTF2022/Forensics/File types
❯ mv flag.lzip.out flag.lzip

picoCTF2022/Forensics/File types
❯ lzip -d flag.lzip
lzip: flag.lzip: Bad magic number (file not in lzip format).
lzip: Deleting output file 'flag.lzip.out', if it exists.

picoCTF2022/Forensics/File types
❯ ls
 flag.ar   flag.cpio   flag.lzip   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag.lzip
flag.lzip: LZ4 compressed data (v1.4+)

picoCTF2022/Forensics/File types
❯ mv flag.lzip flag.lz4

picoCTF2022/Forensics/File types
❯ lz4 -d flag.lz4
Decoding file flag
flag.lz4             : decoded 266 bytes

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   flag.cpio   flag.lz4   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: LZMA compressed data, non-streamed, size 254

picoCTF2022/Forensics/File types
❯ mv flag flag.lzma

picoCTF2022/Forensics/File types
❯ lzma -d flag.lzma

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   flag.cpio   flag.lz4   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: lzop compressed data - version 1.040, LZO1X-1, os: Unix

picoCTF2022/Forensics/File types
❯ mv flag flag.lzo

picoCTF2022/Forensics/File types
❯ lzop -d flag.lzo

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   flag.cpio   flag.lz4   flag.lzo   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: lzip compressed data, version: 1

picoCTF2022/Forensics/File types
❯ mv flag flag.lzip

picoCTF2022/Forensics/File types
❯ lzip -d flag.lzip

picoCTF2022/Forensics/File types
❯ ls
 flag.ar   flag.cpio   flag.lz4   flag.lzip.out   flag.lzo   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag.lzip.out
flag.lzip.out: XZ compressed data, checksum CRC64

picoCTF2022/Forensics/File types
❯ mv flag.lzip.out flag.xz

picoCTF2022/Forensics/File types
❯ unxz flag.xz

picoCTF2022/Forensics/File types
❯ ls
 flag   flag.ar   flag.cpio   flag.lz4   flag.lzo   Flag.pdf   flag.sh   flag_enc

picoCTF2022/Forensics/File types
❯ file flag
flag: ASCII text

picoCTF2022/Forensics/File types
❯ cat flag
7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f37396230316332367d0a
```
The flag is in hexadecimal, i transformed it to string using [cyberchef](https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')&input=NzA2OTYzNmY0MzU0NDY3YjY2MzE2YzY1NmU0MDZkMzM1ZjZkNDA2ZTMxNzA3NTZjNDA3NDMxMzA2ZTVmCjY2MzA3MjVmMzA2MjMyNjM3NTcyMzEzNzc5NWYzNzM5NjIzMDMxNjMzMjM2N2QwYQ).

**The flag is: picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_79b01c26}**
