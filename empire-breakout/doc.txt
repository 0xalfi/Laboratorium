hi folks, in this documentation i wanna share with you about this lab. the empire-breakout lab can be found in vulnhub site.
If you wanna try the lab, just download the file empire-breakout from the original site. So, let's get started..

First, we need to import the .vdi file. A VDI file is a virtual disk image used by Oracle VM VirtualBoxVDI.
And this format is used when create a new virtual machine with a new disk.

Second, open the virtual machine and get the IP. My machine has 192.168.1.15 IP address.
Then, first thing that i do after look up in the machine is login. And how we can login if don't know the username and password.
So, i try to scan all ports the machine using nmap tools. The command that i used is 'sudo nmap -sC -sV -sS -vv 192.168.1.15'
you can see all documentantions of nmap from manual pages 

After scanning, i got some ports open in the machine.
the ports are:
80/tcp
139/tcp
445/tcp
10000/tcp
20000/tcp
