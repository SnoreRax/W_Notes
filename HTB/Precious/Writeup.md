# Tasks

[**1. nginx is running with what additional software designed to serve web applications?**](#task-1)

[**2. Which HTTP response header reveals the underlying scripting Language of the web application?**](#task-2)

[**3. Which Ruby library are the PDF documents generated with?**](#task-3)

[**4. Which 2022 CVE applies to that specific version of pdfkit?**](#task-4)

[**5. Which directory located in the running user's home directory is used by Bundler to store configuration files?**](#task-5)

[**6. User Flag**](#user-flag)

[**7. Which command can henry run with sudo, without providing a password?**](#task-7)

[**8. Which is the name of the file that allows for user-controlled input to the update_dependencies.rb script?**](#task-8)

[**9. Root Flag**](root-flag)

# Tools Used

- nmap
- curl
- exiftool

# Skills Used

- Using cURL to check HTTP response headers
- Grabbing metadata from file
- CVE Exploitation
- Abusing misconfigured binaries

# Solutions

## Task 1

Quite the unusual start, but still basically the same thing, _nmap scan_.

![image](https://github.com/user-attachments/assets/0b889ac4-49c8-4fcc-afdc-98adeccc3d81)

Seems like I didn't get my answer just from this, let's refine the scan further now that we know _port 80_ is open:

**Command:**

```
nmap -sCV -p 80 -vv [IP Address]
```

**Results:**

![image](https://github.com/user-attachments/assets/8b02957f-6001-4eb1-9006-5beec6cceef2)

Just a heads up btw, make sure to add the domain name in your ```/etc/hosts``` so _nmap_ can scan properly.

**Answer: Phusion Passenger**

## Task 2

Use _curl_:

**Command:**

```
curl -s -I http://[Domain Name]
```

**Results:**

![image](https://github.com/user-attachments/assets/011458cc-7945-4dc7-8321-6f69390b219f)

**Answer: X-Runtime**

## Task 3

Checked the hint here, apparently we need to use _exiftool_ to get the file's metadata, should be where we can get info on the _Ruby library_ used for this.

But first, let's upload a valid URL in the website to get our own pdf file:

![image](https://github.com/user-attachments/assets/7d751771-d873-4f66-a710-869069220c20)

Apparently it won't take other URL's, nor itself, weird.

Used ```python -m http.server 8080``` to see if hosting my own server works, apparently it does.

![image](https://github.com/user-attachments/assets/42be89a6-c002-4480-b967-f28715dfe78c)

**P.S.** Ignore my messy directory lmao.

Anyways, locate the file and use ```exiftool``` on it.

![image](https://github.com/user-attachments/assets/e910e93d-15b1-4917-94bd-ac724848e680)

There it is, at the bottom.

**Answer: pdfkit**

## Task 4

Well, google search again.

![image](https://github.com/user-attachments/assets/c8706824-127f-4e99-b569-e6712cec3e0f)

**Answer: CVE-2022-25765**

## Task 5

Aight, time to exploit.

Btw, look at the [**repo**](https://github.com/shamo0/PDFkit-CMD-Injection) I found. What a coincidence that it's the same machine lmao.

Anyways, follow along the steps in that repo, and be sure to properly adjust the _curl request_ accordingly.

Here's what it should like:

![image](https://github.com/user-attachments/assets/e2a8ad01-246e-40e4-8863-21c13d8fd743)

And yes, that's a lot of terminals.

Anyways, now that we have a reverse shell, let's stabilize it so that it doesn't randomly break.

**Process:**

```
1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

Now that we have that, let's search for those _.conf_ files as instructed.

![image](https://github.com/user-attachments/assets/aeaebf44-00ea-435f-b82e-eadb85e6d76f)

Found it.

**Answer: .bundle**

## User Flag

![image](https://github.com/user-attachments/assets/21b90b72-fa79-4ccb-ab6e-132f9e0b1cf1)

Used the credentials we found earlier.

**Answer: 8a7a4f2cc19a46fc17e78be7d994a0a9**

## Task 7

![image](https://github.com/user-attachments/assets/3b72ac05-8f1f-4f8b-a4d6-d63ec913f511)

Simple ```sudo -l``` right here.

**Answer: /usr/bin/ruby /opt/update_dependencies.rb**

## Task 8

![image](https://github.com/user-attachments/assets/0df73d59-474d-4267-8da5-f5a15d3d32d7)

I think seeing that tells us enough.

**Answer: dependencies.yml**

## Root Flag

Found this _PoC_ from [**here**](https://staaldraad.github.io/post/2021-01-09-universal-rce-ruby-yaml-load-updated/) that could help with exploiting the _.yaml_ file.

But, this is where I got confused, thank god for **0xdf** though. 

![image](https://github.com/user-attachments/assets/38109133-1080-427b-adec-42ca57da2d25)

So this is what the result should look like, but I'll explain for a bit as much as I can. I don't understand exactly how the _PoC_ works, but I do know that we want to use _git_set_ for typing commands. Use ```id``` first,
as shown in the original _PoC_, to confirm that the command is reflected after running the binary. Once that's done, I grabbed the specific command that **0xdf** used to copy ```/bin/bash``` into ```/tmp``` and give it
_sudo_ permissions.

This is where I'm confused though, because when trying to use the new shell created after the attack, it must be run with ```-p```, otherwise, you will remain as user _henry_ instead of _root_. Even **0xdf** states that
it must be run with ```-p``` for _root_. To make up for the fact that I looked at his writeup, I'll dig into it more (basically means ask ChatGPT lmao).

![image](https://github.com/user-attachments/assets/2a8d90f5-3e11-41a8-8b02-20655e70859b)

Apparently this is why, ```-p``` is to tell _bash_ not to drop privileges.

Anyways, now that that's out the way, let's get the flag.

![image](https://github.com/user-attachments/assets/bcd015e2-0a86-42c2-82c5-3fd7f79d1374)

**Answer: 13abfa5ccc72b7eb7203876189d317a9**
