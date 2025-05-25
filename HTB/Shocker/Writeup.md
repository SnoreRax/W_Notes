# Tasks

[**1. How many TCP ports are listening on Shocker?**](#task-1)

[**2. What is the name of the directory available on the webserver that is a standard name known for running scripts via the Common Gateway Interface?**](#task-2)

[**3. What is the name of the script in the cgi-bin directory?**](#task-3)

[**4. Optional question: The output from user.sh matches the output from what standard Linux command?**](#task-4)

[**5. What 2014 CVE ID describes a remote code execution vulnerability in Bash when invoked through Apache CGI?**](#task-5)

[**6. What user is the webserver running as on Shocker?**](#task-6)

[**7. User Flag**](#user-flag)

[**8. Which binary can the shelly user can run as root on Shocker?**](#task-8)

[**9. Root Flag**](#root-flag)

# Tools Used

- nmap
- gobuster
- metasploit

# Skills Used

- Enumeration for directories and files within directories
- CVE Exploitation using Metasploit Framework
- Privilege Escalation via misconfigured bin permissions

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/e5cc02f3-a1e8-471d-b553-ab46527271a3)

**Answer: 2**

## Task 2

![image](https://github.com/user-attachments/assets/c83eaf09-2278-4597-b827-4b50bcf53a60)

Nothing here yet lmao, time for _gobuster_

![image](https://github.com/user-attachments/assets/2e10801b-6f97-4c2e-b2c2-44ffcb4aaee8)

Scan's not done, but I figured it was this considering CGI = Common Gateway Interface lol.

**Answer: cgi-bin**

## Task 3

I used the hint, apparently I need to enumerate the _cgi-bin_ using another tool.

It also told me to check for **_common scripts_** such as: ```.cgi, .sh, .pl```.

So yeah, I did that and found this:

![image](https://github.com/user-attachments/assets/2916e298-82f1-46f0-ad5b-c304416471ba)

**Answer: user.sh**

## Task 4

For context, this is what's in _user.sh_:

![image](https://github.com/user-attachments/assets/e4a50e52-36de-4f3f-b17e-3e3a6c74990d)

So, I just copy/pasted into google ```13:38:57 up 28 min,  0 users,  load average: 0.00, 0.00, 0.00``` to see what it was.

Apparently it was a _load average_, so now I search for how to check that in the Linux CLI, and found ```uptime```.

**Answer: uptime**

## Task 5

Just go to _ExploitDB_, here:

![image](https://github.com/user-attachments/assets/a08b6e01-cd53-4a16-b9b4-75df062862ec)

![image](https://github.com/user-attachments/assets/7634968c-8712-4dd4-bd6c-00c76b2d5867)

Anyways there was two, but _CVE-2014-6271_ was the one that worked, so.

**Answer: CVE-2014-6271**

## Task 6

Now before we can run ```whoami``` on the user in the webserver, first we actually have to be able to get a _rev shell_ as that user.

So, use _msfconsole_:

Search the exploit.

![image](https://github.com/user-attachments/assets/a66586a3-01ed-486e-aef4-438b2f94eab3)

Use it and verify.

![image](https://github.com/user-attachments/assets/1f489637-1c6b-4f4c-83c4-cf248bb0856f)

Don't forget to set the options (all of it that says 'yes').

![image](https://github.com/user-attachments/assets/5feb644f-ad70-4183-b187-e911d13e14fd)

Then let it rip.

![image](https://github.com/user-attachments/assets/026e9850-8b67-4252-8fdd-cb66d30b266f)

Anyways, since I forgot that _meterpreter_ doesn't rly use the usual Linux commands, I gotta find out how to run ```whoami``` here lol.

It was apparently called ```getuid``` lmao.

![image](https://github.com/user-attachments/assets/68237bb2-6a44-479a-a116-1340445f1439)

**Answer: shelly**

## User Flag

![image](https://github.com/user-attachments/assets/49196e8c-5e90-4060-9092-84e97cfab476)

Yes.

**Answer: 8f628d46946e9034597835a14cf52826**

## Task 8

Ok so, I know this is supposed to be just ```sudo -l``` (like istg), but idk if _meterpreter_ has any options that's basically ```sudo``` :/

So, I used this:

![image](https://github.com/user-attachments/assets/9e21ea35-483e-43e9-897d-31721acf5e18)

Source: [**This**](https://www.infosecmatter.com/metasploit-module-library/?mm=post/multi/recon/sudo_commands)

After running it, I got this:

![image](https://github.com/user-attachments/assets/85cedf9a-c55f-456d-850e-b4217ff0b2c5)

It's time.

**Answer: perl**

## Root Flag

I feel a bit dumb for this, but apparently I could downgrade the _meterpreter_ shell by simply using ```shell``` :/

I only wanna downgrade it cuz I wanna use ```sudo``` btw, there might be a better way of going about that but I'm not sure, considering the downgraded shell was unstable too lmao.

Anyways, now that I did that, I can get what I need from [**GTFOBins**](https://gtfobins.github.io/) and do this:

![image](https://github.com/user-attachments/assets/29ad8b61-f229-4d32-8038-bd96b6e7f969)

_Yoink_

**Answer: 06bf37d7bcaa1887c685f2d007c92493**
