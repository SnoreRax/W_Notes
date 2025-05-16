# Tasks

[**How many open TCP ports are listening on UnderPass?**](#task-1)

[**What UDP port in the nmap top 100 ports is open on UnderPass?**](#task-2)

[**What email address does the UnderPass server list as it's contact email?**](#task-3)

[**What is the name of the application hosted on the UnderPass webserver?**](#task-4)

[**What is the relative path on the webserver for the operator login page for daloRADIUS?**](#task-5)

[**What is the password for the administrator user on the daloRADIUS application?**](#task-6)

[**What is the clear text password of the svcMosh user on UnderPass?**](#task-7)

[**User Flag**](#user-flag)

[**What is the full path of the command that the svcMosh user can run as any user?**](#task-9)

[**Root Flag**](#root-flag)

# Tools Used

- nmap
- snmp-check

# Skills Used/Learned

- SNMP Enumeration
- OSINT (for docs)
- Even more OSINT (also for docs, but this time for privilege escalation)

# Solution

## Task 1

Command:

```
nmap -sCV [IP Address]
```

Result:

![image](https://github.com/user-attachments/assets/2dd2d11b-aa6c-491f-8512-0403d10cd43f)

**Answer: 2**

## Task 2

So, we can't actually see the _UDP_ ports since we still have to declare that explicitly with ```-sU```, so use this instead:

```
sudo nmap -sU [IP Address] --top-ports=100
```

Anyways, here's the results of the scan:

![image](https://github.com/user-attachments/assets/b4035967-24ff-495b-9417-42165e799d1f)

**Answer: 161**

## Task 3

Use this:

```
snmp-check [IP Address]
```

That is basically an enumeration tool for _SNMP_ (could come in handy in the future).

Result:

![image](https://github.com/user-attachments/assets/88bb062f-3100-435a-bba9-8d990da85298)

**Answer: steve@underpass.htb**

## Task 4

This specifically:

![image](https://github.com/user-attachments/assets/3aa9a5cf-a313-43c9-9eda-1dbea3f62f56)

**Answer: daloradius**

## Task 5

So for this, we need to do a little bit of _OSINT_ apparently, but we can skip ahead to the relevant stuff lmao:

![image](https://github.com/user-attachments/assets/4d6c4c0c-8ccb-4540-9ccb-90b84332866c)

Testing this out on the website:

![image](https://github.com/user-attachments/assets/33210a0c-1ff0-4005-b9a3-8d871219d368)

And there we go.

**Answer: /daloradius/app/operators/login.php**

## Task 6

Use this:

![image](https://github.com/user-attachments/assets/8c8687e7-d066-41aa-87a3-5dc2397a0cba)

Conveniently enough it was right there lmao, must've been a lot of HTB players searching that.

**Answer: radius**

## Task 7

Just searched it lmao:

![image](https://github.com/user-attachments/assets/368aaa7e-8c88-4a15-8f81-03853b067341)

Anyways, it's hashed but I know it's _MD5_.

Don't believe me? Here:

![image](https://github.com/user-attachments/assets/2defcf5a-ae09-48ee-b048-7e26c162d422)

Anyways, time to crack it.

Command:

```
john --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-MD5 [hash.txt]
```

Result:

![image](https://github.com/user-attachments/assets/374b9fd0-5f60-41ce-9926-87df21d2203f)

**Answer: underwaterfriends**

Now we can use that for _ssh_.

## User Flag

Command:

```
ssh svcMosh@[IP Address]
```

Using the password we got earlier, we got this:

![image](https://github.com/user-attachments/assets/681b0626-3464-4334-8752-60624285740f)

**Answer: 71730cfcc4fad3ff1ac62efaf9b84f10**

## Task 9

A simple ```sudo -l``` will do apparently:

![image](https://github.com/user-attachments/assets/e8b42178-1998-4d2f-ae87-f7a9f4111c27)

**Answer: /usr/bin/mosh-server**

## Root Flag

After running the _mosh server_, we found something interesting:

![image](https://github.com/user-attachments/assets/81442f6e-4394-42ac-af1e-bbdf877e4eb2)

If we can connect to that server, we can get _root_, but...

More _OSINT_ with more _docs_...

![image](https://github.com/user-attachments/assets/aa8b7623-4b0c-4baa-a030-46c4b6f15474)

![image](https://github.com/user-attachments/assets/b265df43-55f7-458c-b9d1-76e4628a5f13)

Since we need the key, we'll need to use this:

![image](https://github.com/user-attachments/assets/239f6069-b63a-4237-a152-70efee4c1ed5)

**Note:** Not really relevant but just so you know, that's not the same key I used since I had to reset the server lol.

Anyways, after running this (based on the docs):

```
MOSH_KEY=[Provided Key (from /usr/bin/mosh-server)] mosh-client [Machine IP] 60001
```

We got this:

![image](https://github.com/user-attachments/assets/cea6ac84-8471-4f0e-98f4-18fa38e1ab5f)

**Answer: 136dbaa3ab4d8e5584faa9e33bd743a0**
