---
layout: post
title: How I passed the eCPPTv2
---

Having recently completed the [eLearnSecurity Certified Professional Penetration Tester (eCPPTv2)](https://elearnsecurity.com/product/ecpptv2-certification/), I decided to write a review of the certification, and some useful resources that could help some of you to prepare/pass the exam.

![](/assets/posts/how-i-passed-ecpptv2/ecppt_cert.png)

## What's eCPPTv2?

The eCPPTv2 it's usually the next step after the eJPT. I would say that it's like the OSCP, but from eLearnSecurity.
Before OSCP was updated to include AD, a lot of people were saying that eCPPTv2 is more realistic, and not "forced difficulty", since in the eCPPTv2 you have more time, all the tools are allowed, etc.

eLearnSecurity says that by obtaining the eCPPTv2, your skills in the following areas will be assessed and certified:

- Penetration testing processes and methodologies, against Windows and Linux targets
- Vulnerability Assessment of Networks
- Vulnerability Assessment of Web Applications
- Advanced Exploitation with Metasploit
- Performing Attacks in Pivoting
- Web application Manual exploitation
- Information Gathering and Reconnaissance
- Scanning and Profiling the target
- Privilege escalation and Persistence
- Exploit Development
- Advanced Reporting skills and Remediation

## Course Review

The material provided by [INE](https://my.ine.com/path/9a29e89e-1327-4fe8-a201-031780263fa9) is huge. I had a great experience with INE already doing the [eJPT](https://elearnsecurity.com/product/ejpt-certification/)
So, this time, I decided to pay for 1 year of the Premium subscription, because it's the only way to have access to the labs.
To be honest, I won't recommend this to you. Why? First, because the labs were almost all the time down, the reason is that they are changing the labs to use their web-based virtual machine... and nope... Please, INE this is not the way to go...
I prefer to use my own system, with my own configuration, my own tmux plugins, etc., but as far as I know, this is not going to be possible anymore.

In the course, you will get tons of slides, some videos, etc. In fact, it's a lot of material, and I didn't go through all of it.

It's divided into modules, so the path you will have to follow it's pretty clear.

I recommend it, however, it's not the only platform I used to prepare myself for the exam.

## Exam preparation

Besides, the INE course I can recommend these other resources that I used, and they prepared me really well to pass the exam.

The exam has some areas that you will need to control to pass successfully. The most important ones are Buffer Overflow and pivoting.

### Practice

#### Resources I used to practice Buffer Overflow:

- [bufferoverflowprep](https://tryhackme.com/room/bufferoverflowprep)
  - This one is a must if you don't know how to exploit a basic buffer overflow. You will learn a lot, and it gives you the additional information that combined with the one provided with INE, you will master BoF for the exam.
- [gatekeeper](https://tryhackme.com/room/gatekeeper)
- [brainpan](https://tryhackme.com/room/brainpan)

#### Resources I used to practice Pivoting (plus other things):

Pivoting:
- [wreath](https://tryhackme.com/room/wreath)
  - Do it, and thank me later. It's long, but you will use a lot of these things in the exam.
- [forbusinessreasons](https://tryhackme.com/room/forbusinessreasons)

### Blogs
- [Explore hidden networks with double pivoting](https://pentest.blog/explore-hidden-networks-with-double-pivoting/)
- [Tunneling with chisel and ssf](https://0xdf.gitlab.io/2020/08/10/tunneling-with-chisel-and-ssf-update.html)
- [Enable RDP and Remote Assistance on a remote machine](https://viralmaniar.github.io/internal%20pentest/internal%20infrastructure%20pentest/network%20pentest/Enable-RDP-and-Remote-Assistance-on-a-remote-machine/)

## Exam experience

You have 14 days to complete the certification. 7 days in the lab, and 7 days more to write the report.

The requirements to pass the exam are really clear in the PDF that you can download once you start the exam.
You will need to be `root` or `nt authority\system` in all the machines, but it won't be enough, you have to exploit all the vulnerabilities, and follow the instructions regarding the report.

I started my exam on Saturday at 11:00 am, with a cup of coffee and ready to hack. I took breaks during the day, went for a walk with my dog, etc. I got 3/5 machines on the first day.

I woke up around 09:00 on Sunday, and around 10:00 I was again back to the exam. At 15:00 (approx) I compromised the 2 machines left.

I started the report the same Sunday, and I finished it on Monday evening after work.

In 4 days I got the email saying I passed the exam.

## Conclusion

I can absolutely recommend the certification. It was a great weekend enjoying the exam lab.

Tips:
- Practice the resources I wrote above and enumerate, enumerate and enumerate.
- It's not a CTF. Take a piece of paper to draw the network, and you will be figuring out the steps you have to do next.
- I had to reset the lab once because one exploit was failing. So, I had to repeat some processes to reach the machine I was doing at that moment. Keep this in mind, save screenshots, files, exploits, etc. Everything to redo all the intrusion faster just in case you need to reset the lab.

Cheers!
