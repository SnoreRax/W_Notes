# Tasks

[**1. Which is the highest open TCP port on the target machine?**](#task-1)

[**2. What is the name of the open source software that the application on 55555 is "powered by"?**](#task-2)

[**3. What is the version of request-baskets running on Sau?**](#task-3)

[**4. What is the 2023 CVE ID for a Server-Side Request Forgery (SSRF) in this version of request-baskets?**](#task-4)

[**5. What is the name of the software that the application running on port 80 is "powered by"?**](#task-5)

[**6. There is an unauthenticated command injection vulnerability in MailTrail v0.53. What is the relative path on the webserver targeted by this exploit?**](#task-6)

[**7. What system user is the Mailtrack application running as on Sau?**](#task-7)

[**8. User Flag**](#user-flag)

[**9. What is the full path to the binary (without arguments) the puma user can run as root on Sau?**](#task-9)

[**10. What is the full version string for the instance of systemd installed on Sau?**](#task-10)

[**11. What is the 2023 CVE ID for a local privilege escalation vulnerability in this version of systemd?**](#task-11)

[**12. Root Flag**](#root-flag)

# Tools Used

- nmap

# Skills Used

- CVE Exploitation (a lot)
- That's pretty much it honestly

# Solutions

## Task 1

<img width="945" height="584" alt="image" src="https://github.com/user-attachments/assets/6c6a331f-6877-4b1c-a034-1e922f79f422" />

**Answer: 55555**

## Task 2

<img width="279" height="54" alt="image" src="https://github.com/user-attachments/assets/b4ef7f65-50d3-408a-b2cb-0e14a272d0fc" />

This is what I found upon inspecting the website.

**Answer: request-baskets**

## Task 3

Refer to the image above.

**Answer: 1.2.1**

## Task 4

Ok, as much as I can, I'm gonna try to see if I can understand [**this**](https://github.com/entr0pie/CVE-2023-27163?tab=readme-ov-file) enough to pull it off on my own, in case the PoC doesn't work for me again lol.

Also, I found [**this**](https://github.com/mathias-mrsn/request-baskets-v121-ssrf) lmao. Apparently it was specifically coded for this machine, so yeah.

**Answer: CVE-2023-27163**

## Task 5

Ok so as a continuation of the previous task, here are the things I understood about the exploit:

```
1. The attack takes place in /api/baskets/{name}, where {name} is arbitrary I think.
2. In the request (not exactly sure how it's used), proxy_response is set to true, which I'm assuming is so that you can access what you want to access.
3. Probably the most important bit, but from what I've read so far, the intended target is the localhost on a different port to access content on said port.
```

So with that out of the way, this task makes more sense now, since normally I can't access whatever's on port 80. 

Anyways, since I got an exploit [**specifically coded**](https://github.com/mathias-mrsn/request-baskets-v121-ssrf) for this machine, I'm just gonna use that.

<img width="914" height="105" alt="image" src="https://github.com/user-attachments/assets/dda8689d-3efa-431d-83ff-fbfd054d1414" />

<img width="1920" height="850" alt="image" src="https://github.com/user-attachments/assets/f9da3fdf-02c7-4fab-ba21-1d98fff2eb63" />

Welp, doesn't look pretty, but it got the job done. Either that or the content in port 80 really just looks like this lol.

**Answer: Maltrail**

## Task 6

About that vulnerability, I found [**this**](https://github.com/spookier/Maltrail-v0.53-Exploit).

Considering that there's a login function in the website (but not clickable apparently lol), I figured that the directory in question is ```/login```.

<img width="530" height="77" alt="image" src="https://github.com/user-attachments/assets/d1403056-f153-4000-a830-b0bdcf4e54d3" />

Yep.

**Answer: /login**

## Task 7

Use the vulnerability found earlier.

<img width="585" height="61" alt="image" src="https://github.com/user-attachments/assets/acc13321-abd8-42ef-a765-98544d61f0a4" />

Like so.

<img width="487" height="153" alt="image" src="https://github.com/user-attachments/assets/68736e10-13d3-4874-8baf-d1788b206550" />

Now that we have reverse shell, let's stabilize.

**Answer: puma**

## User Flag

First, I'm gonna do this for convenience:

```
1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

Then, I'm just gonna go to the ```/home``` directory to get the user flag.

<img width="273" height="189" alt="image" src="https://github.com/user-attachments/assets/b811a83a-e134-43ff-a582-b9d8fa79e5e7" />

**Answer: 3a161728f7e56d2a3105985b3ae41c69**

## Task 9

Run ```sudo -l``` as per usual.

<img width="749" height="112" alt="image" src="https://github.com/user-attachments/assets/aaf1d586-b72d-4929-aff0-2e305470ee54" />

I can already see where this is going with making a new service that's malicious lol.

**Answer: /usr/bin/systemctl**

## Task 10

<img width="947" height="78" alt="image" src="https://github.com/user-attachments/assets/0a0eed08-e66e-4728-a0ca-0e2a991a6b20" />

Here it is.

**Answer: systemd 245 (245.4-4ubuntu3.22)**

## Task 11

Ok, maybe the exploit doesn't go as I thought it would lol. Let's see though.

<img width="734" height="765" alt="image" src="https://github.com/user-attachments/assets/edea1071-c998-4d98-b885-e2b8f2e27e11" />

Ok technically it kind of is, but it's not really a malicious service.

Anyways, [**here's**](https://medium.com/@zenmoviefornotification/saidov-maxim-cve-2023-26604-c1232a526ba7) where I found it, apparently it's really simple.

**Answer: CVE-2023–26604**

## Root Flag

So, just as instructed in the article, just run ```!sh``` while running the service as **sudoer** to exit and get a shell as **root**.

<img width="661" height="487" alt="image" src="https://github.com/user-attachments/assets/c60e8faf-8f7a-47b9-a3fe-95d60575bfde" />

Now that we have that, let's get the flag.

<img width="270" height="114" alt="image" src="https://github.com/user-attachments/assets/e15ab08e-bd97-4e91-b963-30b7f33611d3" />

**Answer: a41c037a845d6bec0558fb7b688c36df**
