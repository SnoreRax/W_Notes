# Tasks

[**1. We're prompted for log on credentials when accessing the target over HTTP. What username is disclosed when looking at the HTTP response headers?**](#task-1)

[**2. Weak passwords are all too common and this target is no exception. What is the password for this target's login?**](#task-2)

[**3. There are several kinds of files that are commonly dropped into a file share to target other users who may browse to the share. If the user browses to the share, 
their host will try to authenticate to the attacker. What is the file extension that can be uploaded here to trigger that connection?**](#task-3)

[**4. We've intercepted an Net-NTLMv2 hash with Responder. What is the mode in Hashcat required to crack this hash format?**](#task-4)

[**5. What is the tony user's password?**](#task-5)

[**6. User Flag**](#user-flag)

[**7. What is the filename that stores the command history for PowerShell for tony?**](#task-7)

[**8. Looking at the Powershell history we can see some actions being performed with a specific printer type. Research of this should show that it's exploitable for privilege escalation and a module is available for the Metasploit framework. What is the module name?**](#task-8)

[**9. Initial attempts to exploit this vulnerability may fail under specific logon types such as non-interactive, but in this scenario we can switch to an interactive logon. What command can be used in Metasploit to switch to an interactive logon process?**](#task-9)

[**10. **](#task-10)

[**11. Root Flag**](#root-flag)

# Tools Used

- nmap
- curl
- hashcat
- metasploit

# Skills Used

- 

# Solutions

## Task1

Ok, this is an unsual first task. Regardless, I'm still running that _nmap_ scan.

<img width="759" height="621" alt="image" src="https://github.com/user-attachments/assets/5db28ff0-5475-43fc-90ef-efabdfd45d81" />

Keep that in mind for later, as per usual.

Anyways, let's see what the target greets us with.

<img width="509" height="305" alt="image" src="https://github.com/user-attachments/assets/79868263-1bb4-4310-bcff-9bf218dda391" />

So from this, I'm supposed to get the HTTP response headers to see if there's a username that'll be revealed, so I'll use _curl_.

<img width="752" height="457" alt="image" src="https://github.com/user-attachments/assets/c18b054c-de36-45fc-ba4e-3cbda89a9c00" />

There it is.

**Answer: admin**

 ## Task 2

 That task description is way too obvious. 

 **Answer: admin**

 ## Task 3

 <img width="1162" height="328" alt="image" src="https://github.com/user-attachments/assets/d9a7e667-76a0-4d67-bdf0-3e7c696f9b57" />

~~This page gives me a massive hint. Let's follow that lead.~~

That was not it lol. The question itself was way too verbose, so I kinda got mislead. Focus on the word **file share**, and you should know what to do. Also, do remember that in the _nmap_ scan, ```port 445``` is open.

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/80b80105-b6d4-44f1-9f7c-d710ded3d18d" />

Now that's the right answer.

**Answer: SCF**

**P.S.** Use [**this guide**](https://nored0x.github.io/red-teaming/smb-share-scf-file-attacks/), it'll be useful. Also ironically it's also using this machine to showcase it lmao.

## Task 4

Just check the manual.

<img width="369" height="596" alt="image" src="https://github.com/user-attachments/assets/b761287c-73e5-4a5f-8933-13f6c1b9e969" />

Kinda interesting though that the task explicitly suggests using _hashcat_ for hash cracking, and not _john_, lol.

**Answer: 5600**

## Task 5

To get that, remember [**that guide**](https://nored0x.github.io/red-teaming/smb-share-scf-file-attacks/) earlier? Here's what it looks like.

**Step 1:**

<img width="404" height="142" alt="image" src="https://github.com/user-attachments/assets/f795d620-b059-4847-b8cd-1eb08996d514" />

Create the **SCF** file that will get the hash.

**Step 2:**

<img width="1489" height="632" alt="image" src="https://github.com/user-attachments/assets/76696da0-3dee-4e48-9abf-f7288e0cb74f" />

Upload the **SCF** file to the share to intercept hashes.

**Step 3:**

You can use either **responder** which [**0xdf**](https://0xdf.gitlab.io/2022/02/26/htb-driver.html#shell-as-tony) used or **impacket-smbserver** which the guide I referenced earlier uses.

Make sure to set it up **before** uploading the **SCF** file

<img width="653" height="207" alt="image" src="https://github.com/user-attachments/assets/b17ae891-d518-442a-b406-ca4fe334072b" />

**Step 4:**

<img width="942" height="460" alt="image" src="https://github.com/user-attachments/assets/d2f6d541-c64e-4f23-9c47-5539e2031d80" />

Stonks.

Anyways, now that we have the hash, time to use **hashcat**.

<img width="946" height="503" alt="image" src="https://github.com/user-attachments/assets/50b2ec50-bde4-47f8-b877-477e405d9898" />

There we go.

**Answer: liltony**

## User Flag

To establish a shell to the target, we'll use _evil-winrm_. You've used this like last year, so it's been a while. Here's a review.

<img width="942" height="515" alt="image" src="https://github.com/user-attachments/assets/8fd179f1-c763-4077-be40-ab40374c672a" />

You can use that as a guide, anyways.

<img width="947" height="229" alt="image" src="https://github.com/user-attachments/assets/53c4a0d9-8a47-4da1-bf60-38aeac0273e8" />

Now that we're in, let's look for that flag.

<img width="468" height="238" alt="image" src="https://github.com/user-attachments/assets/cb7be1a2-698f-4a89-ad21-640c178b0e2d" />

**Answer: 0b9054c28bd817021bb66b9c2d8003db**

**P.S.** I did not know _evil-winrm_ lets me use **CLI** commands lmao. Though limited since it can't really run a command's respective flags.

## Task 7

Searched it in the internet lol.

<img width="723" height="169" alt="image" src="https://github.com/user-attachments/assets/2683353d-4221-4db3-9435-4bd58b74a083" />

Seek and ye shall find.

**Answer: ConsoleHost_history.txt**

## Task 8

Like the task said, it's in _Metasploit_. Before we search it there though, I wanna search for the details about the exploitable printer.

<img width="1143" height="494" alt="image" src="https://github.com/user-attachments/assets/2c56e509-7159-457e-92ea-a996c8f0b655" />

Found the details [**here**](https://www.pentagrid.ch/en/blog/local-privilege-escalation-in-ricoh-printer-drivers-for-windows-cve-2019-19363/).

Anyways, about that _Metasploit_ module, look at this [**documentation**](https://github.com/rapid7/metasploit-framework/blob/master/documentation/modules/exploit/windows/local/ricoh_driver_privesc.md).

<img width="437" height="263" alt="image" src="https://github.com/user-attachments/assets/9258d41e-345b-42f1-821b-a74ca49a929d" />

There it is. I'll show me using it after this task.

**Answer: ricoh_driver_privesc**

## Task 9

I'm not sure why the tasks love to skip over telling you to do certain parts before getting there, so I'll just show it here rq.

