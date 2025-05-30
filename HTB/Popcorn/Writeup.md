# Tasks

[**1. How many TCP ports are listening on Popcorn?**](#task-1)

[**2. What is the relative path on the webserver to a file sharing service?**](#task-2)

[**3. What HTTP request header is being used to filter uploaded content?**](#task-3)

# Tools Used

-

# Skills Used

-

# Solutions

## Task 1

Same old same old.

![image](https://github.com/user-attachments/assets/13005bfa-46d2-4b71-b491-edb0acf14991)

**Answer: 2**

## Task 2

First, let's put the _domain name_ inside of our _/etc/hosts_.

![image](https://github.com/user-attachments/assets/bf0e6fb9-3c87-48c8-9a23-036d237fbadf)

Just like that.

Anyways, let's see if we can manually enumerate this before automating it.

![image](https://github.com/user-attachments/assets/48c4f955-1647-43e8-8dfd-f88e4b3caf5a)

Guess not :/

Aight let's pull out _gobuster_.

**Command Used:**

```
gobuster dir -u http://popcorn.htb/ -w /usr/share/dirb/wordlists/common.txt -x php 
```

Again, you can use other wordlists, I just like to use _common.txt_ most of the time.

Anyways, here's what I found from the _gobuster_ scan:

![image](https://github.com/user-attachments/assets/f51ece80-b7b4-4fce-8f88-1afccf274db4)

After checking out some of the other endpoints, I found this:

![image](https://github.com/user-attachments/assets/6f1d03f5-fea0-492a-baa1-bb1df4b568e0)

**Answer: /torrent**

## Task 3

