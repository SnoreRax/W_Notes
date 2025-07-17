# Tasks

[**1. How many TCP ports are open on SwagShop?**](#task-1)

[**2. What is the name of the eCommerce Software being used by the website?**](#task-2)

[**3. Which version of Magento is in use?**](#task-3)

[**4. The Magento Shoplift vulnerability can be used to change the credentials of what user?**](#task-4)

[**5. There are multiple different methods to use this admin access to get remote code execution on SwagShop through Magento. What user is the web server running the site as?**](#task-5)

[**6. User Flag**](#task-6)

[**7. Which binary can www-data run as root without a password?**](#task-7)

[**8. Root Flag**](#root-flag)

# Tools Used

- nmap
- gobuster

# Skills Used

- Directory fuzzing
- CVE exploitation for initial access
- File upload exploitation
- Abusing misconfigured binaries

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/a76628d2-3471-4cb8-955a-315e1edca5ae)

**Answer: 2**

## Task 2

![image](https://github.com/user-attachments/assets/8a6b9bc6-e02e-4c0c-9f7b-dba3b3426d27)

**Answer: Magento**

## Task 3

Not sure how to go about it, so I used the hint and ran a _gobuster_ scan.

![image](https://github.com/user-attachments/assets/64664772-b4e8-4dce-8511-cbf21d539b2b)

After which, I scoured each directory for anything useful and found what I needed.

![image](https://github.com/user-attachments/assets/fdd81a23-1e07-4fda-a7fb-49a7384376d3)

**Answer: 1.9.0.0**

## Task 4

Ok honestly I got stuck in this for way too long, another overthinking/reading comprehenion moment lmao.

So, I found this _github_ page for the exploit:

![image](https://github.com/user-attachments/assets/e3ef4eab-3154-40c7-a896-1b057e08162f)

After that, I spent quite a while searching for the username being edited. I kept typing in usernames of accounts that are added after the attack, such as _defaultmanager_ and _ypwq_.

However, I forgot that we were trying to find the username being **edited**, not the username being **added**.

So...

**Answer: admin**

## Task 5

![image](https://github.com/user-attachments/assets/0fbc71ef-cfc4-4a37-a6f6-01756cda6e75)

Found this page after a while. I already ran the exploit found earlier, and here's what happened:

![image](https://github.com/user-attachments/assets/f917a8aa-ca47-43b0-a48f-1f8135d19c3d)

Now I can use that to login:

![image](https://github.com/user-attachments/assets/26f085a7-7762-4ddc-bb40-5a430b2e12ea)

Still need to find a way to get a foothold though.

Anyways, I tried to use the **Magento CE < 1.9.0.1 - (Authenticated) Remote Code Execution** exploit I found using _searchsploit_, and even searched writeups on how to use that since mine kept breaking. But then,
I came to the unfortunate conclusion that I can't really use it cuz Python is bloody broken on my Kali machine :/

So, I went to search for another method to keep myself sane.

In another writeup by [**0xRick**](https://0xrick.github.io/hack-the-box/swagshop/), I found that you could just enable _symlinks_ and do a classic _PNG File Upload Attack_ to get reverse shell. Refer to the writeup for more details, I'm just relieved that I finally got in lmao.

<img width="881" height="267" alt="image" src="https://github.com/user-attachments/assets/0f872dda-46f7-4ac4-904e-46359a1ba662" />

There we are.

**Answer: www-data**

## User Flag

<img width="353" height="262" alt="image" src="https://github.com/user-attachments/assets/eef1a682-dba7-4327-a7d5-655a5ee35129" />

Here you go.

**Answer: 6ce151034c59bee3d8ac4ec8af657b7a**

## Task 7

Ok first, I'm just gonna stabilize the shell real quick.

```
1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

Much better. Anyways, let's just run ```sudo -l``` for this.

<img width="746" height="131" alt="image" src="https://github.com/user-attachments/assets/7ecd05d9-9b56-491d-b5d6-1b91d315457b" />

And there we go.

**Answer: vi**

## Root Flag

Just go [**here**](https://gtfobins.github.io/gtfobins/vi/#sudo) for the exploit lol. Or you can refer to the screenshot down here.

<img width="644" height="96" alt="image" src="https://github.com/user-attachments/assets/7baa8249-144d-4626-baf9-b526d3fdce65" />

Anyways.

**Answer: 4ac3b4b0ad0560bfe98d4face4d47273**
