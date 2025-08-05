# Tasks

[**1. We're prompted for log on credentials when accessing the target over HTTP. What username is disclosed when looking at the HTTP response headers?**](#task-1)

[**2. Weak passwords are all too common and this target is no exception. What is the password for this target's login?**](#task-2)

[**3. There are several kinds of files that are commonly dropped into a file share to target other users who may browse to the share. If the user browses to the share, 
their host will try to authenticate to the attacker. What is the file extension that can be uploaded here to trigger that connection?**](#task-3)

[**4. We've intercepted an Net-NTLMv2 hash with Responder. What is the mode in Hashcat required to crack this hash format?**](#task-4)

# Tools Used

- nmap
- curl
- 

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

## Task 4

Just check the manual.

<img width="369" height="596" alt="image" src="https://github.com/user-attachments/assets/b761287c-73e5-4a5f-8933-13f6c1b9e969" />

Kinda interesting though that the task explicitly suggests using _hashcat_ for hash cracking, and not _john_, lol.

**Answer: 5600**

## Task 5

