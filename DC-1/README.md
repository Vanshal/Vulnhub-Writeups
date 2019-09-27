<h1>DC-1 Write-up</h1>

<h3>PORT 80</h3>

Drupal 7 Exploit : https://www.exploit-db.com/exploits/34992
This create's a admin user named "vanshal" and with password "vanshal"

 ```python 34992.py -t http://192.168.56.46/ -u vanshal -p vanshal```
 
 ![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-1/images/drup7exp.png)

<h3>Drupal to shell</h3> 

Follow this blog: https://www.sevenlayers.com/index.php/164-drupal-to-reverse-shell 
![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-1/images/revshell.png)

<h3>Privilege Escalation :</h3>

**Flag1:** 
Location : /var/www

**Every good CMS needs a config file - and so do you.**

**DB credentials :**

Found Creds in setting.php

/var/www/sites/default/settings.php

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-1/images/configpass.png)

**FLAG2**                                                                                                 
 * Brute force and dictionary attacks aren't the                                                         
 * only ways to gain access (and you WILL need access).                                                  
 * What can you do with these credentials?                                                               

I can directly read the flag 4 from the user flag4

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-1/images/flag4.png)

<h4>THEFINALFLAG (PRIV ESC)</h4>

<h4>Exploiting find SUID</h4>

find . -exec "/bin/sh" \;



![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-1/images/FINALFLAGROOT.png)
