# Tasks

[**1. How many TCP ports are listening on PermX?**](#task-1)

[**2. What is the default domain name used by the web server on the box?**](#task-2)

[**3. On what subdomain of permx.htb is there an online learning platform?**](#task-3)

[**4. What is the name of the application running on `lms.permx.htb?**](#task-4)

[**5. What version of Chamilo is running on PermX?**](#task-5)

[**6. What is the name of the 2023 CVE ID for a stored cross-site scripting vulnerability that leads to remote code execution?**](#task-6)

[**7. What user is the webserver running as on PermX?**](#task-7)

[**8. What is the full path to the file that holds configuration data to include the database connection information for Chamilo?**](#task-8)

[**9. User Flag**](#user-flag)

[**10. What is the full path to the script that the mtz user can run as any user without a password?**](#task-10)

[**11. ```/opt/acl.sh``` allow for changing the access control list on file in what directory? (Don't include the trailing / on the directory.)**](#task-11)

[**12. Does ```setfacl``` follow symbolic links by default?**](#task-12)

[**13. Root Flag**](#root-flag)

# Tools Used

- nmap
- ffuf
- curl
- linpeas

# Skills Used

- Subdomain enumeration via FFUF
- Exploitation using PoC from CVE
- File upload via cURL
- System Enumeration via LinPeas
- Abusing misconfigured ACL file to change content in other files

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/abe67321-d2be-4504-b696-d3c3323cbd01)

**Answer: 2**

## Task 2

It led me straight to _permx.htb_ so.

**Answer: permx.htb**

## Task 3

For this, we'll use _ffuf_ for subdomain enumeration.

**Command:**

```
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/namelist.txt -u http://FUZZ.permx.htb/
```

**Results:**

![image](https://github.com/user-attachments/assets/cd30c264-b0f5-44f3-9902-c5fe56de17f5)

Looks like that didn't work cuz I forgot to add the ```-H``` flag, oops.

**Command:**

```
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://10.10.11.23 -H "HOST: FUZZ.permx.htb" -ac
```

Used another wordlist to see if it'd work better. **ALSO,** don't forget about the ```-ac``` flag, saves a lot of trouble by automatically filtering out useless subdomains.

**Results:**

![image](https://github.com/user-attachments/assets/7bd14233-065a-4e71-bc82-879bad9c7943)

Don't forget to put the new subdomains in ```/etc/hosts```:

![image](https://github.com/user-attachments/assets/20977efc-7dec-4f9a-9370-3e9b97c90162)

And there we go, found it.

![image](https://github.com/user-attachments/assets/913185fd-7538-46ef-a0c9-e2f6a46216d8)

**Answer: lms.permx.htb**

## Task 4

It's in the screenshot of the site earlier lol.

**Answer: Chamilo**

## Task 5

I don't see anything that could help with this, so I'm just gonna go fuzz this real quick.

**Command:**

```
ffuf -u http://lms.permx.htb/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

**Results:**

![image](https://github.com/user-attachments/assets/39430ea0-9ecc-4f42-87bd-f0033fddd050)

Now let's check a bunch of these lmao.

![image](https://github.com/user-attachments/assets/006cd28a-84b1-44dd-8baa-b85dadef1d3a)

And ofc, I went for _robots.txt_ as soon as I saw it lol. _REAME.txt_ seems interesting tho.

![image](https://github.com/user-attachments/assets/c1f3c780-916c-40e3-bd17-25da8f720af0)

It's not there :/

Let's try _/documentation/_.

![image](https://github.com/user-attachments/assets/ae86399b-d85e-46a6-8f28-3366bfd44a9c)

There it is. And ofc, considering the task is to find the service version, there probably a _CVE_ involved.

**Answer: 1.11**

## Task 6

Yep I called it lmao.

Just search google for it. Found it [**here**](https://pentest-tools.com/vulnerabilities-exploits/chamilo-lms-11124-remote-code-execution_22949) though.

![image](https://github.com/user-attachments/assets/65119201-3014-431f-a39a-3316e06e165a)

**Answer: CVE-2023-4220**

## Task 7

So [**here**](https://github.com/Ziad-Sakr/Chamilo-CVE-2023-4220-Exploit) we can see how the attacks works, so lemme just grab that and use it.

![image](https://github.com/user-attachments/assets/40b8afce-4deb-4e4b-9a27-9c82cc5485a4)

![image](https://github.com/user-attachments/assets/3c16f243-5253-4ce9-a105-7dfc82ed7804)

Ok so I might be wrong on that end of trying to use it directly. It should work perfectly fine, but that's assuming that it's not attacking a subdomain. If it is, it runs into troubles uploading it into the right directory.

**Lesson Learned:** Whenever you come across labs that exploit _CVEs_ again, understand how it works. The attack shown (if not exploited using _Metasploit_) is usually just a _PoC_, so you're gonna have to make adjustments.

![image](https://github.com/user-attachments/assets/0f95c7b6-a242-4aca-baae-33c1e5d47a11)

On that note, take a look at this section. It tells you exactly how to exploit it yourself, so grab a [**php revshell**](https://www.revshells.com/) using _PHPPentestMonkey_ and get to work.

![image](https://github.com/user-attachments/assets/0030bc6b-ae9b-4e8a-bc41-32895585486e)

![image](https://github.com/user-attachments/assets/2e97a97a-404e-48d9-b347-34ffc2365d90)

Aight, now just get _netcat_ listening and we should be good to go.

![image](https://github.com/user-attachments/assets/0a6356bd-b03f-479b-896f-17d911853e48)

**Answer: www-data**

## Task 8

Before we continue, let's stabilize the shell first (for convenience lmao).

```
Stabilization process:

1. python3 (or python) -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

![image](https://github.com/user-attachments/assets/b9ab9700-eafd-4598-90c3-f39f19fd0031)

Anyways, since I gotta find a configuration file, let's get _LinPeas_ in here to automate the process.

**Host Machine:**

![image](https://github.com/user-attachments/assets/86309a0e-07bc-4090-93f9-d60a0d754827)

**Victim Machine:**

![image](https://github.com/user-attachments/assets/b5bb381b-3b75-4e74-a4e5-cd7aa06fb9fc)

Now time to run it.

![image](https://github.com/user-attachments/assets/1538e40f-9ae4-436b-9aa4-514b218d9acb)

Time to wait for the results.

**A few minutes later...**

![image](https://github.com/user-attachments/assets/3278d8be-7eba-41bb-b278-90b312d1b987)

There's more but that's the only one I need to find for now for the task so...

**Answer: /var/www/chamilo/app/config/configuration.php**

## User Flag

Got the other user's password here:

![image](https://github.com/user-attachments/assets/2a4d4712-e2cb-42b3-93d2-608ffeaf3a6d)

Anyways, just switch user to that one now and get the flag.

![image](https://github.com/user-attachments/assets/c6174b77-8dbf-4db9-90cf-668f3d3f731d)

**Answer: 437474ccdd9812bc7cc4efd5c56d4050**

## Task 10

Simple ```sudo -l``` should do.

![image](https://github.com/user-attachments/assets/bd316e32-86ee-458d-9cbf-8fc1b9855d5a)

**Answer: /opt/acl.sh**

## Task 11

Check the file contents and you'll get what you need.

![image](https://github.com/user-attachments/assets/2e974832-d2a4-432a-8f1b-9ad6b5cf10e1)

**Answer: /home/mtz**

## Task 12

![image](https://github.com/user-attachments/assets/779bca40-92ad-4f52-9d28-51423b499a5d)

After looking at this description in the manual for ```setfacl```, I assume that means yes.

**Answer: yes**

## Root Flag

For this part, I got really confused on the whole concept for _ACLs_ and _symbolic links_, so I used [**0xdf's writeup**](https://0xdf.gitlab.io/2024/11/02/htb-permx.html) as a reference on how the exploit works.

I'm not entirely sure how the exploit works, but from my limited understanding, the _ACL_ gives permissions on the stuff that I can do, but the ```acl.sh``` file is misconfigured, since making a _symbolic link_ to
any other file on the system, say ```/etc/sudoers``` or ```/etc/passwd``` and putting that in ```/home/mtz``` can allow me to change the permissions of my current user on that file, like _rwx_. For more details on the
exploit, just read along the writeup so you can understand more, but in this case, I chose to explot ```/etc/sudoers``` to give me unlimited _sudo perms_.

**Setting symbolic link to ```/etc/sudoers``` and setting perms with ```/opt/acl.sh```:**

![image](https://github.com/user-attachments/assets/bd8cefed-5a61-4921-9e8d-dbd49bf84a9b)

**Giving mtz user unli sudo perms:**

![image](https://github.com/user-attachments/assets/31b8af77-bc6b-4461-956d-2c9cfaf09f60)

![image](https://github.com/user-attachments/assets/6b9eaa69-7863-4354-b3ef-02ae7f9822b6)

**P.S.** This took me wayyyyyyyyyy too long to figure out lmao. I was breaking the shell so much and kept getting a new one each time XD. I hate this machine :/

And FINALLY, after all that, I can get the damn flag. I definitely need to read that writeup more properly.

**Answer: 34367c94f54eeb521e3a59827969ff9b**
