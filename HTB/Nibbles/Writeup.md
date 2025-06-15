# Tasks

[**1. How many open TCP ports are listening on Nibbles?**](#task-1)

[**2. What is the relative path on the webserver to a blog?**](#task-2)

[**3. What content management system (CMS) is being used by the blog??**](#task-3)

[**4. What is the relative path to an XML file that contains the admin username?**](#task-4)

[**5. What is the admin user's password to log into the blog?**](#task-5)

[**6. What version of nibble blog is running on the target machine? Do not include the "v".**](#task-6)

[**7. What is the 2015 CVE ID for an authenticated code execution by file upload vulnerability in this version of NibbleBlog.**](#task-7)

[**8. Which user the Nibbleblog instance is running on the target machine?**](#task-8)

[**9. User Flag**](#user-flag)

[**10. What is the name of the script that nibbler can run as root on Nibbles?**](#task-10)

[**11. Enter the permission set on monitor.sh? Use the Linux file permissions format, like -rw-rw-r--.**](#task-11)

[**12. Root Flag**](#root-flag)

# Tools Used

- nmap
- gobuster
- ffuf
- metasploit

# Skills Used

- Recursive directory enumeration via FFUF
- CVE Exploitation via Metasploit
- Abusing file misconfigurations

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/d728554f-a366-4d35-a655-6190f117f10a)

**Answer: 2**

## Task 2

Time to use _gobuster_ (I may have been too lazy to bother finding _php_ files btw lol):

![image](https://github.com/user-attachments/assets/9d1180c9-d194-4ef3-9bd5-4f5620fac882)

Aight welp, that found nothing, so let's check page source, see if there's any comments.

![image](https://github.com/user-attachments/assets/d939a851-721a-49ed-9c4c-9741babdb64e)

Yep, there in fact, was.

![image](https://github.com/user-attachments/assets/180b3f35-4d59-41ad-8c33-799925243bc7)

**Answer: /nibbleblog**

## Task 3

![image](https://github.com/user-attachments/assets/79c5688a-9547-4cf1-af17-dbd396efc6b5)

Says it here lol.

**Answer: nibbleblog**

**P.S.** Seeing as how we need to find out what this is, I'm just gonna call it out now, it's a **CVE Exploit**.

## Task 4

Alright, now I'm running _gobuster_, but with ```-x php``` this time and in _/nibbleblog/_ to try and find the _XML_ file. For context, clicking on _Atom_ on the bottom left led me to _feed.php_, which was an _XML_.

![image](https://github.com/user-attachments/assets/33adb61e-f18f-458c-8d3b-2185e2e1c174)

Didn't find much, looked at the hint and apparently it's better if I search **recursively**, so instead, I tried using _ffuf_.

**Command:**

```
ffuf -u http://[IP]/nibbleblog/FUZZ -w /usr/share/dirb/wordlists/common.txt -e .php -recursion -recursion-depth [arbitrary] -t 50
```

**Results:**

This took a while btw lol, went afk for a while too.

**Nearly 2 hours later**

I'm not even gonna bother showing the entire result, it's a lot. I honestly should've piped it into an output file :/

![image](https://github.com/user-attachments/assets/4cb163bc-21d7-41a8-b885-a8da3c261193)

Btw, this is just part of the scan, I had to manually check a bunch of directories for a while to find what I needed, and apparently I should've included the _XML file type_, cuz I found what I needed:

![image](https://github.com/user-attachments/assets/f26aaf78-35d8-4b11-b49c-7f4628630138)

Finally.

**Answer: /nibbleblog/content/private/users.xml**

## Task 5

Ok so, before I could use _hydra_, I checked the hint just to make sure I was on the right track...

![image](https://github.com/user-attachments/assets/0f060665-bfa9-4571-af15-fad67f2befb6)

It outright gave me the answer :/

Welp, so much for that huh.

**Answer: nibbles**

## Task 6

Found it in the settings page.

![image](https://github.com/user-attachments/assets/c95c47fc-0d90-42c9-8ca4-2c05f123576a)

**Answer: 4.0.3**

## Task 7

I called that from a mile away btw lmao. We rly are using a _CVE_.

![image](https://github.com/user-attachments/assets/a7aa708a-dc14-4282-a627-c90c25e4203d)

Forgot I had to get the _CVE ID_ tho so let's go to _ExploitDB_ to find that.

![image](https://github.com/user-attachments/assets/e0e39bd0-5fd5-442d-ab55-d9f32e026aa6)

Here it is.

**Answer: CVE-2015-6967**

## Task 8

Aight, time to exploit.

![image](https://github.com/user-attachments/assets/b88d5116-d881-48a5-b7d9-cfbd3d95fedd)

![image](https://github.com/user-attachments/assets/d20c65a2-ef3a-4f4b-a849-ae529d44a287)

![image](https://github.com/user-attachments/assets/9ac77272-f541-4c0c-8779-42e718e1f4e1)

**Answer: nibbler**

## User Flag

![image](https://github.com/user-attachments/assets/87c97d6e-9588-4f8c-8cc2-61d4ebc2a254)

**Answer: 184ca13c3217efe8f5e1b02b9b3bf09b**

## Task 10

![image](https://github.com/user-attachments/assets/972f770c-7bda-40e2-8c82-9c8d32ee901c)

Gotcha.

**Answer: monitor.sh**

## Task 11

![image](https://github.com/user-attachments/assets/edc91a5f-e5e7-4d55-bca0-505202e50e5e)

**Answer: -rwxrwxrwx**

## Root Flag

Replace the script with our own.

![image](https://github.com/user-attachments/assets/7f495798-47f1-4e5b-8836-ac2e3af92c41)

Then run it as _sudo_ to establish reverse shell.

![image](https://github.com/user-attachments/assets/b7fc838d-ba0c-4554-83bc-da3deb882cc9)

![image](https://github.com/user-attachments/assets/6ef4d600-c953-499d-b400-6128353c40d3)

And there we have it.

**Answer: eb49c59ba454249b00789d61bf0ae40e**
