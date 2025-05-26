# Tasks

[**1. What is the name of the FTP software running on Blocky?**](#task-1)

[**2. What username is given by enumerating the website?**](#task-2)

[**3. What relative path on the webserver offers two JAR files for download?**](#task-3)

[**4. What password is present in the BlockCore.jar file?**](#task-4)

[**5. User Flag**](#user-flag)

[**6. Is notch able to run sudo -i and get a shell as root?**](#task-6)

[**7. Root Flag**](#root-flag)

# Tools Used

- nmap
- gobuster
- jd-gui

# Skills Used

- Simple website recon
- Directory enumeration
- Decompiling JAR file

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/78fd7307-4904-4b7e-90b0-f46c7e3b81d0)

Yes.

**Answer: ProFTPD**

## Task 2

After accessing the website and checking it around for a bit, I found a blog post here:

![image](https://github.com/user-attachments/assets/3b4324c4-009c-4a8c-8595-b263aaee7cb9)

I tried to see if the author was the user, and apparently it was.

**Answer: Notch**

## Task 3

Ok so at first, I thought it was these two:

![image](https://github.com/user-attachments/assets/804eb61e-07b8-4082-a915-c3c80758a9f8)

Since they downloaded files onto my machine after clicking on them.

But, after looking at the files themselves, they weren't in a _.jar_ format, and checking the contents also revealed that it was just _html_ code.

So after checking the hint, apparently I have to run _gobuster_ to find it. Probably not gonna use _common.txt_ though since the hint did tell me to use a big wordlist so yeah.

![image](https://github.com/user-attachments/assets/afc2f33d-8dce-4c36-83e0-943242f45cbc)

So I found all these, now time to check each and every one of them that's **NOT** status 400.

![image](https://github.com/user-attachments/assets/0ebf4443-db87-46e2-9c52-6aa70a7931a5)

Anyways, there it is.

**Answer: /plugins**

## Task 4

Apparently I need to use something called a _jar decompiler_ to check the contents of the jar file I got, so I used _jd-gui_ and got this:

![image](https://github.com/user-attachments/assets/ddd80eb1-9e99-4100-86b4-fc87669d97ca)

**Answer: 8YsqfCTnvxAUeduzjNSXe22**

## User Flag

After getting the credentials, I thought that the password was for root, considering where I found the credentials.

Apparently though, the password was for user _notch_:

![image](https://github.com/user-attachments/assets/1c361ec3-31ae-4eec-b1db-a68f2a1b622d)

So now, I got the flag.

**Answer: **0876dc5e78e67a24d2ea02f99c33cfce**

## Task 6

Considering the question, I naturally thought of using ```sudo -l``` to check our permissions, given that we also have the password.

![image](https://github.com/user-attachments/assets/d4944f79-935f-4ab6-b92f-b398e56b0133)

0_0

Well would you look at that, _notch_ IS root (and rightfully so).

**Answer: yes**

## Root Flag

![image](https://github.com/user-attachments/assets/7000cf11-68a8-4971-a67f-11b5f68f53de)

Yep.

**Answer: 3d1e0f1066ac3ab98206d669d8e89e1b**
