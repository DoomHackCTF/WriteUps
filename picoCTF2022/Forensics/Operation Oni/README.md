<img src="img/challenge.png">

We have a .img file, so the first thing i did was to mount it.
```shell
┌──(kali㉿kali)-[~/Documents]
└─$ fdisk -l disk.img       
Disk disk.img: 230 MiB, 241172480 bytes, 471040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x0b0051d0

Device     Boot  Start    End Sectors  Size Id Type
disk.img1  *      2048 206847  204800  100M 83 Linux
disk.img2       206848 471039  264192  129M 83 Linux
┌──(root㉿kali)-[/home/kali/Documents]
└─# mount -o loop,offset=105906176 disk.img /mnt/  
```
After that i went to the root folder and saw the files.
```shell
┌──(root㉿kali)-[/home/kali/Documents]
└─# cd /mnt      
                                                                                        
┌──(root㉿kali)-[/mnt]
└─# ls
bin   dev  home  lost+found  mnt  proc  run   srv  tmp  var
boot  etc  lib   media       opt  root  sbin  sys  usr
                                                                                        
┌──(root㉿kali)-[/mnt]
└─# cd root 
                                                                                        
┌──(root㉿kali)-[/mnt/root]
└─# ls
                                                                                        
┌──(root㉿kali)-[/mnt/root]
└─# ls -la
total 4
drwx------  3 root root 1024 Oct  6 10:30 .
drwxr-xr-x 21 root root 1024 Oct  6 10:28 ..
-rw-------  1 root root   36 Oct  6 10:31 .ash_history
drwx------  2 root root 1024 Oct  6 10:30 .ssh
                                                                                        
┌──(root㉿kali)-[/mnt/root]
└─# cat .ash_history 
ssh-keygen -t ed25519
ls .ssh/
halt
```
As we can see a ssh key was generated and possibly is in the .ssh/ directory, so i changed to that directory.
```shell
┌──(root㉿kali)-[/mnt/root]
└─# cd .ssh 
                                                                                        
┌──(root㉿kali)-[/mnt/root/.ssh]
└─# ls    
id_ed25519  id_ed25519.pub
                                                                                        
┌──(root㉿kali)-[/mnt/root/.ssh]
└─# ls -la 
total 4
drwx------ 2 root root 1024 Oct  6 10:30 .
drwx------ 3 root root 1024 Oct  6 10:30 ..
-rw------- 1 root root  411 Oct  6 10:30 id_ed25519
-rw-r--r-- 1 root root   96 Oct  6 10:30 id_ed25519.pub
                                                                                        
┌──(root㉿kali)-[/mnt/root/.ssh]
└─# cat id_ed25519  
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLAAAAJgxpYKDMaWC
gwAAAAtzc2gtZWQyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLA
AAAECItu0F8DIjWxTp+KeMDvX1lQwYtUvP2SfSVOfMOChxYGCtd7hso2E7OQItY6aTjMMy
KZb1FVmeBfnVjyHcGYosAAAADnJvb3RAbG9jYWxob3N0AQIDBAUGBw==
-----END OPENSSH PRIVATE KEY-----
                                                                                        
┌──(root㉿kali)-[/mnt/root/.ssh]
└─# cat id_ed25519.pub 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGCtd7hso2E7OQItY6aTjMMyKZb1FVmeBfnVjyHcGYos root@localhost
```
We have both the public and private key, the last step is connecting to the server we where given when we initialized the instance with the private key.
```shell
┌──(root㉿kali)-[/mnt/root/.ssh]
└─# ssh -i id_ed25519 -p 53188 ctf-player@saturn.picoctf.net 
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.13.0-1017-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ctf-player@challenge:~$ ls
flag.txt
ctf-player@challenge:~$ cat flag.txt 
picoCTF{k3y_5l3u7h_d6e19567}ctf-player@challenge:~$ Connection to saturn.picoctf.net closed by remote host.
Connection to saturn.picoctf.net closed.
```



