---
layout: post
title: How I passed the eJPT
---

Having recently completed the [eLearnSecurity Junior Penetration Tester (eJPT)](https://elearnsecurity.com/product/ejpt-certification/)
certification, I decided to write this post with some useful commands/techniques that could help some of you to prepare/pass the exam, but first, let's write down a little review of the certification.

![](/assets/posts/how-i-passed-ejpt/ejpt_certification.png)

## Course Review

Due to my background, I just took a quick look at the material, but I skipped a lot.
I saw too many presentations/powerpoints/slides/whatever, so I went quickly through all of them, and I jumped to the black box labs.

The material provided by [INE](https://my.ine.com/path/a223968e-3a74-45ed-884d-2d16760b8bbd) is superb (and free).
I skipped some parts, but this depends on your previous experience. If you are a junior, then I don't recommend following my path.

I went through all the 3 black box labs, and they are IMO harder than the exam (at least in my case).
The exam’s funny! There are 20 questions to answer, and 15 correct answers are required to pass.
I started the exam at 22:00, I read all the questions first, and then I started to answer the questions while I found the correct answers in the exam lab.
I finished at 02:00.

eJPT has been my first infosec certification, and it seemed to me "easy" (compared to HackTheBox boxes (easy/medium) level for example) but realistic scenario, so I enjoyed that!
I don't have any other certification to compare though, please don't get me wrong.

Do I recommend the certification? Yes, for sure! :) Buy your voucher and enjoy it!

## Commands/techniques Summary

### Routing

```bash
ip route add ROUTETO via ROUTEFROM
```

### Enumeration

#### Ping sweep

```bash
fping -a -g 10.10.10.0/24 2>/dev/null
nmap -sn 10.10.10.0/24 -oN hosts
```

#### Scans

##### Full

```bash
cat hosts | grep for | awk {'print $5'} > ips
nmap -sC -sV -n -p- -T5 -iL ips --open -oN portScan 
```

##### UDP "Quick"

```bash
nmap -sU -sV 10.10.10.10
```

### Network Attacks

##### Brute forcing with Hydra

```bash
hydra -L users -P passwords 10.10.10.10 ftp
```

##### Windows Shares Using Null sessions

```bash
enum4linux -a 10.10.10.10
smbclient \\\\10.10.10.10\\share -N
```

##### Exploit

```bash
searchsploit xxxxx
```

You can use here Metasploit if you like.

### System Attacks

```bash
unshadow passwd shadow > hashes
john --wordlist=/path/wordlist hashes
```

### Web Applications

##### Directory and file scanning

```bash
wfuzz -c --hc=404 -t 200 -w /usr/share/dirbuster/directory-list-2.3-medium.txt http://10.10.10.10/FUZZ
```

##### SQLMap

```bash
sqlmap -u 'http://10.10.10.10/xxxx.php?xx=x' --dbs
sqlmap -u 'http://10.10.10.10/xxxx.php?xx=x' -D database --tables
sqlmap -u 'http://10.10.10.10/xxxx.php?xx=x' -T table --dump
```

## Conclusion

I enjoyed a lot, and as I already said, I think it's a good starting point. If you decide to buy the voucher, good luck!

Cheers!