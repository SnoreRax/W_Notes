# Tasks

[**1. What version of Nginx is running?**](#task-1)

[**2. What JavaScript file is referenced in the page's header?**](#task-2)

[**3. When downloading a file on the internal web application there are three parameters in the POST request. They are photo, filetype, and what?**](#task-3)

[**4. Which of the three parameters is vulnerable to a blind command injection?**](#task-4)

[**5. User Flag**](#user-flag)

[**6. Which bash script may be run as root by the wizard user through sudo?**](#task-6)

[**7. Which binary is "enabled" in the /opt/.bashrc file?**](#task-7)

[**8. Root Flag**](#root-flag)

# Tools Used

- nmap
- burpsuite

# Skills Used

- Source code inspection
- Using BurpSuite to intercept and change requests
- Abusing misconfigured PATH variable

# Solutions

## Task 1

<img width="763" height="514" alt="image" src="https://github.com/user-attachments/assets/7de355e3-884e-4c36-8175-abe32c4afc6f" />

Yes.

**Answer: 1.18.0**

## Task 2

<img width="511" height="211" alt="image" src="https://github.com/user-attachments/assets/47f9d25c-46f5-4591-93ce-383d9c549508" />

Didn't think it was that simple ://

**Answer: photobomb.js**

## Task 3

<img width="823" height="176" alt="image" src="https://github.com/user-attachments/assets/b50850a3-b363-43ab-aa36-11c7d669782b" />

This is what was in the **photobomb.js** file btw, credentials. Used that to login since the redirect in the landing page needed credentials beforehand.

<img width="1920" height="806" alt="image" src="https://github.com/user-attachments/assets/85ce8273-f3b0-4c78-a622-2827cb6b1061" />

Now this is where the task's questions actually come in. No need to view the **POST Request** via _curl_, the answer's right there.

**Answer: dimensions**

## Task 4

Apparently one of those parameters are susceptible to **command injection**, but it's **blind** ://

Maybe I do have to intercept the **POST Request**, but with _Burpsuite_.

<img width="1916" height="837" alt="image" src="https://github.com/user-attachments/assets/d131577e-77ac-4534-84e2-66cdb42051f9" />

So I searched online how to tell if **blind command injection** works, I found that using ```sleep+5``` works because that makes the response from the host delayed.

Anyways, I tried the other parameters, but the one that worked was **filetype**.

**Answer: filetype**

## User Flag

Knowing that, I grabbed a shell from [**here**](https://www.revshells.com/) and made sure to URL encode the payload. Tried a bunch btw, the one that worked for me was **Python 3 Shortest**

**Payload:**

<img width="1920" height="848" alt="image" src="https://github.com/user-attachments/assets/7ac01f81-1e28-47f5-865c-2df665acc527" />

**Execution:**

<img width="1634" height="292" alt="image" src="https://github.com/user-attachments/assets/e2678333-023e-4ad9-a802-ba991f9a6b69" />

And of course, don't forget to setup the _netcat_ listener.

Btw before I grab the flag, I'm gonna stabilize the shell first.

```
1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. export TERM=xterm
3. CTRL + Z
4. stty raw -echo;fg
```

Now I'll go find the flag.

<img width="285" height="224" alt="image" src="https://github.com/user-attachments/assets/76dc48c9-e399-4dc1-93f6-79164b79968d" />

**Answer: 94182ab1f6dfb5ea89f050aa060aff10**

## Task 6

Just a simple ```sudo -l``` should do.

<img width="746" height="129" alt="image" src="https://github.com/user-attachments/assets/bda8c481-05c5-4ef7-9d3b-c8c023bccec3" />

**Answer: /opt/cleanup.sh**

## Task 7

<img width="570" height="44" alt="image" src="https://github.com/user-attachments/assets/5f9f2e12-10e3-40cf-8a7e-7128d7190224" />

This one took me a while, had to use a writeup cuz I was confused at what I was looking at lmao. Apparently the answer was **'['** :/

**Answer: [**

## Root Flag

And because I looked at writeups for that, I know that the exploit is about abusing the file path since it's not absolute. Apparently it had something to do with what was supposed to be found in [**Task 7**](#task-7), so yeah.

Anyways, since I wasn't that familiar with **Path Hijacking**, I used [**0xdedinfosec's writeup**](https://0xdedinfosec.vercel.app/blog/hackthebox-photobomb-writeup) as a reference on how I should do it. I'll be sure to
add this to my notes, but below was how I did mine based off of his writeup:

<img width="482" height="75" alt="image" src="https://github.com/user-attachments/assets/a6261cc2-aade-4b78-974b-a13a2bd8dd09" />

Btw, I used **'['** since I thought that might've been it cuz of the previous task lol, other writeups used other bins. 

As far as I understand, I basically just need to pick a file, have it execute a _shell_, give it **execute** perms, then reference that file path while using the bin that's misconfigured. I'll add more details in my notes,
right now, let's get the flag.

<img width="289" height="83" alt="image" src="https://github.com/user-attachments/assets/ede0be8a-13d2-47ae-830b-f2a9490b95cb" />

**Answer: b64fe75b1f1f7ed59d9bc3542631e4c4**
