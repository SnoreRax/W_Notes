# Tasks

[**1. How many open TCP ports are listening on Dog?**](#task-1)

[**2. What is the name of the directory on the root of the webserver that leaks the full source code of the application?**](#task-2)

[**3. What is the CMS used to make the website on Dog? Include a space between two words.**](#task-3)

[**4. What is the password the application uses to connect to the database?**](#task-4)

[**5. What user uses the DB password to log into the admin functionality of Backdrop CMS?**](#task-5)

[**6. What system user is the Backdrop CMS instance running as on Dog?**](#task-6)

[**7. What system user on Dog shares the same DB password?**](#task-7)

[**8. User Flag**](#user-flag)

[**9. What is the full path of the binary that the johncusack user can run as any user on Dog?**](#task-9)

[**10. ```bee``` requires a root directory to run properly. What is the appropriate root directory on Dog? Include the trailing /.**](#task-10)

[**11. What is the bee subcommand to run arbitrary PHP code?**](#task-11)

[**12. Root Flag**](#root-flag)

# Tools Used

- nmap
- ffuf
- git-dumper

# Skills Used

- Directory fuzzing
- Dumping git repos with git-dumper
- CVE exploitation
- Password reuse
- Abusing misconfigured binaries

# Solutions

## Task 1

<img width="765" height="533" alt="image" src="https://github.com/user-attachments/assets/3560138d-014f-4ee4-be4b-d8050212c8b3" />

Just the usual.

**Answer: 2**

## Task 2

So I was gonna use _ffuf_ for directory brute-forcing, but I just noticed that the _nmap_ scan actually included a few interesting directories. Specifically **robots.txt** to view the full details.

<img width="515" height="752" alt="image" src="https://github.com/user-attachments/assets/df47d34b-a337-4243-959a-31a3fc8029b3" />

**Spoiler Alert:** That wasn't it lmao. I didn't notice, but **.git** was found in the _nmap_ scan.

<img width="757" height="524" alt="image" src="https://github.com/user-attachments/assets/74322ddf-82ca-43ea-8ea8-4c2a87f17d6b" />

**Answer: .git**

## Task 3

<img width="250" height="136" alt="image" src="https://github.com/user-attachments/assets/a386d8aa-8d0a-42f8-bdd0-653b87343e45" />

It's just at the bottom of the website lmao.

**Answer: Backdrop CMS**

## Task 4

Used the hint cuz I got lost lol. Apparently I was supposed to use ```git-dumper``` got grab the **.git** of the website.

**How to use:**

```
git-dumper [URL] [DIR]
```

**Example:**

```
git-dumper http://10.10.11.58/.git/ ~/.git
```

Directory's there just ~~in case the **.git** directory is nested for some reason~~ ok so that was a lie, apparently it was to specify where the **.git** repo would be downloaded to and I just learned that the hard way :/

Anyways, now what I fixed that.

<img width="456" height="303" alt="image" src="https://github.com/user-attachments/assets/a8226fd0-01fe-4f32-a14f-bc0950227750" />

Let's go look for that password.

<img width="778" height="410" alt="image" src="https://github.com/user-attachments/assets/e0f89338-a38b-4d36-9900-94c2c741de28" />

Found it here, could be useful too since it's root.

**Answer: BackDropJ2024DS2024**

## Task 5

Ok so apparently it wasn't **root** lmao. Still gotta find the username. There are users listed in the home page, but none of them worked. Then I went to the 'About' Page, and found the email format. Since I have the **.git** repo, I could use this to do the job:

```
grep -R '@dog.htb' *
```

<img width="803" height="72" alt="image" src="https://github.com/user-attachments/assets/d17d062a-9906-457d-a3c7-46eb22461117" />

All I found was **tiffany** lmao, let's try that.

<img width="1920" height="696" alt="image" src="https://github.com/user-attachments/assets/d8cf235f-9763-4db0-8437-af5513ebefff" />

That worked.

**Answer: tiffany**

## Task 6

Ok ig it's time to exploit, let's see what we're working with.

<img width="941" height="203" alt="image" src="https://github.com/user-attachments/assets/1b362474-24f1-4e7d-a1f7-f7416589f0f7" />

Used _searschploit_ to see if there are any available exploits we may be able to use, the **1.27.1 - RCE** exploit looks interesting, so I'll search for that.

<img width="1780" height="355" alt="image" src="https://github.com/user-attachments/assets/07863fa0-1673-48ca-b601-14e91dbe05af" />

Looks like this might work. 

From what I understand, it exploits the fact that this version doesn't properly check **.zip** files that are being uploaded. But, the file needs to have to components. First, the **.info** file that shows that it's a **Backdrop CMS zip file**, and second, the actual shell file that'll be stored in a **PHP** file.

**Shell.info file contents**

<img width="588" height="313" alt="image" src="https://github.com/user-attachments/assets/ba382378-d545-4793-9b09-b3d60d22e843" />

As for the **PHP** shell file, we'll just use **PentestMonkey**.

Anyways, now that I did that, I think this is the page where I upload it:

<img width="1920" height="778" alt="image" src="https://github.com/user-attachments/assets/8f8cd974-300c-4def-bfdb-b7a5bb316eb0" />

I'll just host my own **Python HTTP server** to see if I can upload the **zip** file that way.

<img width="489" height="170" alt="image" src="https://github.com/user-attachments/assets/c1263379-7cec-4950-b4af-3ab67a205bbf" />

<img width="863" height="384" alt="image" src="https://github.com/user-attachments/assets/f3a80339-7c72-4137-ab04-76b3e4ce2f56" />

There we go, now let's see if this works.

Also apparently you can see the exact **Backdrop CMS version** in the **Functionality** page lmao, I did not know that. It does reinforce though that this **CVE** should work, considering it's the exact same version.

<img width="762" height="93" alt="image" src="https://github.com/user-attachments/assets/847bb34b-9245-4aa1-b1ea-1773d3bc0941" />

Ok so, after some troubles with my attempt at manual exploitation, I decided to just grab the exploit from [**here**](https://www.exploit-db.com/exploits/52021) and make the necessary adjustments lmao. It also pointed me to where the exploit will be at after I ran it.

<img width="785" height="137" alt="image" src="https://github.com/user-attachments/assets/2c4f08eb-760a-473d-bb0c-35bd65846bd6" />

~~So yeah. Anyways, since this is a web shell, let's upgrade to a **reverse shell**.~~

**Spoilers:** The web shell is not interactive ://

So instead of going down that rabbit hole, I decided to simply change the **shell script** inside the **CVE script** because I have no idea how they were able to set the file path for the shell ://///

Anyways, I finally got it though at least.

<img width="827" height="209" alt="image" src="https://github.com/user-attachments/assets/25d35478-d52b-44a2-85f2-4e646c8d6019" />

Anyways, just run ```id``` or ```whoami``` and yeah.

**Answer: www-data**

## Task 7

First, I'm gonna stabilize the shell.

```
1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

Now that that's done, I looked at the **home** directory to check the other users.

<img width="188" height="41" alt="image" src="https://github.com/user-attachments/assets/9bc6359b-3e89-4fc0-b941-a3c9626ec4e1" />

Now considering the question, I'm just gonna try the password on both and see which works.

<img width="274" height="115" alt="image" src="https://github.com/user-attachments/assets/261d535c-73ec-428a-894e-539d7addb9d1" />

Ok it was him lmao.

**Answer: johncusack**

## User Flag

<img width="293" height="120" alt="image" src="https://github.com/user-attachments/assets/5f09c7b4-10ac-4700-9ef0-416f9b73189d" />

Yeah.

**Answer: 136ecae01c0cc6ebccc8224b0c59beb8**

## Task 9

Just run ```sudo -l```, you already have the password anyway.

<img width="746" height="150" alt="image" src="https://github.com/user-attachments/assets/4922c3ba-855e-4667-9681-b55f0b02cfe0" />

**Answer: /usr/local/bin/bee**

## Task 10

Ok so for this, I was confused and refered to a writeup after 10 mins. Lo and behold, I simply had a mental block lmao, forgot that these are **bins** we're talking about, so there should be a **man** page to see how to use it.

<img width="949" height="427" alt="image" src="https://github.com/user-attachments/assets/19e7f650-8533-4624-ae30-3bf500a1c359" />

Now that I understand what the question meant, the answer is probably ```/var/www/html/```, since that's usually where the webpage is stored.

<img width="517" height="82" alt="image" src="https://github.com/user-attachments/assets/dddc8067-fe9c-4254-84a2-c26454404818" />

Yep.

**Answer: /var/www/html/**

## Task 11

<img width="948" height="130" alt="image" src="https://github.com/user-attachments/assets/89e36b16-94f3-493b-8c70-8e94179dfe69" />

Yeah just do that lol.

**Answer: eval**

## Root Flag

Now that we have all that, ~~I'm gonna refer to [**revshells.com**](https://www.revshells.com/) again for the **PHP revshell script**~~.

Ok so I don't actually need that, read a writeup and I forgot that since you're running **PHP scripts** directly, I can just use ```system()``` to run what I want, so:

**Command:**

```
sudo /usr/local/bin/bee eval "system('/bin/bash')"
```

**Additonal Notes:**

1. I'm already in the ```/var/www/html``` directory, so I don't need to use ```--root``` to specify where to run it from.
2. I don't think I needed to list the full path, but I just did so just in case lmao.

Anyways, here's the flag:

<img width="267" height="117" alt="image" src="https://github.com/user-attachments/assets/0852a768-283d-4ede-9040-6768028f88d2" />

**Answer: 7a7bdc30f6b4011dcd13956f595289ef**
