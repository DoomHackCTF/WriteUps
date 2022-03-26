# Milkshake 250
##### This is a cyberchef special combo "milkshake"! Please don't hate me!
##### 4e3255334e5449784d6a45794e54566a4e7a49794e5463314e474d334f444d774e3249315a6a51334e6a497a4d4463794e6a59334e5451304e47553d

The message is encoded in hex, base64, hex again and rot 47.
```shell
❯ echo "4e3255334e5449784d6a45794e54566a4e7a49794e5463314e474d334f444d774e3249315a6a51334e6a497a4d4463794e6a59334e5451304e47553d" | xxd -r -p | base64 -d | xxd -r -p | tr ‘\!-~’ ‘P-~\!-O’
OFPPT-CTF{I_L0v3_C7Fs}⏎ 
```
