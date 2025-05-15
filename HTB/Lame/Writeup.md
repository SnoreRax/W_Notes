# Tasks

[**How many of the nmap top 1000 TCP ports are open on the remote host?**](#task-1)

[**What version of VSFTPd is running on Lame?**](#task-2)

[**There is a famous backdoor in VSFTPd version 2.3.4, and a Metasploit module to exploit it. Does that exploit work here?**](#task-3)

[**What version of Samba is running on Lame? Give the numbers up to but not including "-Debian".**](#task-4)

[**What 2007 CVE allows for remote code execution in this version of Samba via shell metacharacters involving the SamrChangePassword function when the "username map script" option is enabled in smb.conf?**](#task-5)

[**Exploiting CVE-2007-2447 returns a shell as which user?**](#task-6)

[**User & Root Flag**](#user-&-root-flag)

# Solutions

## Task 1

**Command:**

``` 
nmap -sCV [IP Address]
```

**Result:**

![image](https://github.com/user-attachments/assets/527e8795-6a8d-4253-a429-d1ee0dfa3bc0)

**Answer: 4**

## Task 2

Refer to previous image, it's there ://

**Answer: 2.3.4**

## Task 3

To figure that out, let's check metasploit for the exploit and try it ourselves:

![image](https://github.com/user-attachments/assets/4349a0c8-11a9-4e94-97b2-6b51ebe5a251)

![image](https://github.com/user-attachments/assets/9775c574-9e7b-41e4-8ba6-6aafbc4712a6)

Anyways, that's a hard nope.

**Answer: no**

## Task 4

Again, refer back to the _nmap scan_ in task 1.

**Answer: 3.0.20**

## Task 5

Search in _metasploit_ again (it's an old _CVE_, it should be there):

![image](https://github.com/user-attachments/assets/cf97ea4b-3da1-4a75-a55f-93a54a0c344e)

And it's _RCE_ too, but I forgot that doesn't give the full _CVE_ name so...

![image](https://github.com/user-attachments/assets/cec19eff-d61f-462d-9939-16d3099d6a3d)

**Answer: CVE-2007-2447**

## Task 6

Aight, time to use this smb exploit:

![image](https://github.com/user-attachments/assets/478eaa0a-7bcb-4a9f-9906-7d14ee633e2b)

Waiting...

Ok on that note, apparently it was done pretty much instantly, but the shell is just unstable, so I thought it was still loading :///

Anyways, here.

**Answer: root**

## User & Root Flag

**User: 9176578a162d09f0f55fbb8b6157e680**

**Root: 4f42e7f9aebb00bae29957bd9872d4d9**
