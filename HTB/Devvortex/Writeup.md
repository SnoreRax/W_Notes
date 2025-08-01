# Tasks

[**1. How many open TCP ports are listening on Devvortex?**](#task-1)

[**2. What subdomain is configured on the target's web server?**](#task-2)

[**3. What Content Management System (CMS) is running on dev.devvortex.htb?**](#task-3)

[**4. Which version of Joomla is running on the target system?**](#task-4)

[**5. What is the 2023 CVE ID for an information disclosure vulnerability in the version of Joomla running on DevVortex?**](#task-5)

[**6. What is the lewis user's password for the CMS?**](#task-6)

[**7. What table in the database contains hashed credentials for the logan user?**](#task-7)

[**8. What is the logan user's password on DevVortex?**](#task-8)

[**9. User Flag**](#user-flag)

[**10. What is the full path to the binary that the logan user can run with root privileges using sudo?**](#task-10)

[**11. What is the 2023 CVE ID of the privilege escalation vulnerability in the installed version of apport-cli?**](#task-11)

[**12. Root Flag**](#root-flag)

# Tools Used

- nmap
- ffuf
- mysql

# Skills Used

- Subdomain enumeration via ffuf
- Directory fuzzing
- Accessing vulnerable files with critical information
- CVE exploitation
- Template shell injection
- Hash cracking

# Solutions

## Task 1

<img width="760" height="353" alt="image" src="https://github.com/user-attachments/assets/ff3d1a0f-609b-4f54-96d3-a4d2a24309d5" />

**Answer: 2**

## Task 2

Here's the _ffuf_ scan for **subdomain enumeration**:

<img width="941" height="496" alt="image" src="https://github.com/user-attachments/assets/fc01a2ed-84a4-4640-a8d5-6b6f555fab42" />

**Answer: dev.devvortex.htb**

## Task 3

So after running _ffuf_ again, but this time for **directory fuzzing**, here's what I found:

<img width="754" height="544" alt="image" src="https://github.com/user-attachments/assets/c9dd23c8-a140-4259-b5b1-bc710f7fda40" />

That ```robots.txt``` directory looks nice, so I checked that too.

<img width="494" height="501" alt="image" src="https://github.com/user-attachments/assets/18562752-f8c4-4908-8d55-f4ac6c201196" />

And there it is.

**Answer: joomla**

## Task 4

~~So the normal way of searching for the file that lists its version doesn't work for some reason (it's in ```/administrator/manifests/files/joomla.xml``` btw, don't know why it won't load though :/), so I resorted to using ```joomscan```, a tool specifically for enumerating **joomla** services that I found in a few writeups because I got confused.~~

**Edit:** I tried it again the next day, now I can view the **.xml** file :/

<img width="946" height="849" alt="image" src="https://github.com/user-attachments/assets/3a213065-d106-45f1-90c1-09ea3ea9388b" />

**Answer: 4.2.6**

**Note:** Feel free to still use ```joomscan``` though. Should be useful for future boxes/labs using **joomla**.

## Task 5

Just search ```joomla 4.2.6 cve``` and you should be able to find it within the first few results.

**Answer: CVE-2023-23752**

## Task 6

Used [**this blog**](https://www.vulncheck.com/blog/joomla-for-rce) here to get an idea on how the **CVE** works. Apparently there's a vulnerable endpoint that pretty much leaks user information for whoever accesses it.

<img width="758" height="728" alt="image" src="https://github.com/user-attachments/assets/94dba660-74d1-4f45-b22b-d9921bc348cd" />

See?

Anyways, we want the _lewis_ user, so yeah go get his credentials and let's login.

**Answer: P4ntherg0t1n5r3c0n##**

## Task 7

<img width="1920" height="778" alt="image" src="https://github.com/user-attachments/assets/f4362292-f96a-47b0-b224-03c6f27b8ea3" />

I logged in now, so let's look around.

**A few minutes later**

<img width="1920" height="769" alt="image" src="https://github.com/user-attachments/assets/394aaad8-97f9-4571-8212-f47cdf42c30a" />

I found a ```templates``` page where I can edit a bunch of **.php** files. You know what that means B)

Anyways, I tested out which files I can edit first though just to make sure, apparently I can only edit ```error.php``` and ```offline.php```, but one should do.

Unfortunately at the time of writing this, it was a little too late for me to screenshot how I got my shell lmao, so just try to picture it. In either ```error.php``` or ```offline.php``` (I used _offline.php_), change the script a little bit to give yourself a reverse shell. I used _PHP PentestMonkey's shell_ that I got [**here**](https://www.revshells.com/), and save it. After that, navigate to it respective directory (it can be seen while editing) and you should be able to get **reverse shell**.

Now that I got it, time to stabilize.

```
1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

<img width="886" height="523" alt="image" src="https://github.com/user-attachments/assets/f73ee691-3a0e-47cc-9532-856a899d7947" />

Now that it's stabilized, let's find that database table with the hashed credentials.

<img width="566" height="187" alt="image" src="https://github.com/user-attachments/assets/70f1de06-6f4e-4f19-81d4-ef5e8e1ab88e" />

Let's see if that helps.

That did not lmao. But...

<img width="389" height="251" alt="image" src="https://github.com/user-attachments/assets/60187b95-a33e-46a3-bb3e-6e2cf999ce2b" />

Remember this? Yeah, you can use that for _mysql_.

<img width="652" height="297" alt="image" src="https://github.com/user-attachments/assets/40f62ad2-4797-4e73-949e-5b6195ab37aa" />

Now let's go through this real quick.

<img width="951" height="477" alt="image" src="https://github.com/user-attachments/assets/8001ca7a-7596-4ba8-9d1c-6960495a1053" />

Ok I know it's jumbled but it's still there lmao.

Anyways, still gotta answer the task.

**Answer: sd4fg_users**

## Task 8

I tried using _hashcat_, but it took too long lmao. Also, I just realized that the website I used to identify the hash type could actually decrypt the hash too, so...

<img width="1663" height="359" alt="image" src="https://github.com/user-attachments/assets/171fb110-b443-4f76-a3d2-af3c1326c1f9" />

I just used that instead lmao.

**Answer: tequieromucho**

## User Flag

<img width="270" height="133" alt="image" src="https://github.com/user-attachments/assets/409a4af0-adfc-4b90-9abf-0210fa832fd9" />

Yep.

**Answer: d6eb803cb1e8ef0b22c6cf47a66ceaa5**

## Task 10

Use ```sudo -l``` as per usual.

<img width="746" height="186" alt="image" src="https://github.com/user-attachments/assets/d6b8bf28-9190-4c22-8d7b-2464c9e807ef" />

**Answer: /usr/bin/apport-cli**

## Task 11

If I didn't see the question for the task, I don't think I would've figured out that this was vulnerable to a **CVE**. Definitely putting that down in my notes.

<img width="833" height="785" alt="image" src="https://github.com/user-attachments/assets/8e7de4ef-c3c5-4b2e-a1f1-8b2b7561dc53" />

Anyways, that's a lot of results lmao.

**Answer: CVE-2023-1326**

## Root Flag

Check ~~[**this**](https://github.com/diego-tella/CVE-2023-1326-PoC) out~~ [**this**](https://vk9-sec.com/cve-2023-1326privilege-escalation-apport-cli-2-26-0/) is better lmao. Apparently the **CVE** is real simple lol.

<img width="872" height="719" alt="image" src="https://github.com/user-attachments/assets/c1d97908-337d-42b7-96a9-ba38be6e275d" />

See?

Now lemme just...

<img width="646" height="559" alt="image" src="https://github.com/user-attachments/assets/664e5e49-34fd-489c-86cd-a43c46693427" />

Yep. Just follow along the blog brudda.

**Answer: 6e789b1bb776b36fd7b2ba1c211ce65a**
