# Tasks

[**1. Which Linux distribution is the target machine running?**](#task-1)

[**2. What version of TLS is the web application on TCP port 443 using?**](#task-2)

[**3. What is the name of the software that's hosting a webserver on 443?**](#task-3)

[**4. Which Elastix endpoint is vulnerable to a Local File Inclusion?**](#task-4)

[**5. What is the name of the FreePBX configuration file that contains the database configuration?**](#task-5)

[**6. What additional flag is needed when attempting to SSH as root to the target machine due to a "no matching key exchange method found" error? It starts with -o and ends with -sha1.**](#task-6)

[**7. User Flag**](#user-flag)

[**8. Root Flag**](#root-flag)

[**9. Beyond Root**](#beyond-root)

# Tools Used

- nmap
- curl
- exploitDB
- ssh

# Skills Used

- Directory Fuzzing
- Exploiting known CVEs
- Exploiting LFI vulnerabilities

# Solutions

## Task 1

Just the usual ```nmap -sCV```:

![image](https://github.com/user-attachments/assets/56c2ca7a-bff7-450a-bafa-ff64d029d8a4)

**Answer: CentOS**

## Task 2

First, I have to rollback _TLS version_ that _Firefox_ uses on my end so I can access the webpage in _about:config_

![image](https://github.com/user-attachments/assets/17ee3cfc-288f-4329-84d2-bb4a42ee528e)

After telling the browser that I know what I was doing, I can now access the page.

![image](https://github.com/user-attachments/assets/c3e9bd56-0a34-4588-9afd-4ec18420670e)

Thing is, we're not here for the page, we're here for the _TLS_ version being used, so we use this:

```
curl -v [URL]
```

```-v``` is basically just verbose, it'll let us see the _TLS_ version being used by the webserver.

![image](https://github.com/user-attachments/assets/1bb09fee-31bf-461d-9663-a90cfdd338ff)

And there it is.

**Answer: 1.0**

## Task 3

Well I mean it's in the webpage and the _nmap_ scan so :/

**Answer: Elastix**

## Task 4

Now for this one, we'll use the usual ```gobuster``` command, but:

![image](https://github.com/user-attachments/assets/9bdc2c23-39e9-4312-9d6f-bd58b61fabad)

The certificate can't be verified, so instead, let's run it with ```--no-tls-validation```:

![image](https://github.com/user-attachments/assets/9eb08c6a-0fe3-4181-a51b-439e3d3b2bf2)

Still can't tell which of these directories it is, so I check the hint for any leads.

Apparently I'm supposed to use an old _CVE_ that uses _graph.php_, so I'll try finding that.

So after finding the endpoint itself, I realized that _gobuster_ was not actually needed at all lmao.

The _CVE_ shows a _PoC_ of how the exploit works, via this directory: _/vtigercrm/graph.php_

**Lesson Learned:** Don't fully rely on _gobuster_ for finding the directory you need, sometimes, you just need to get a _CVE_ see and how it works

**Answer: /vtigercrm/graph.php**

## Task 5

**P.S.** I may have done [**Task 6**](#task-6) before this one by accident because of me wanting to use the exploit already hehe.

You can just see that here:

![image](https://github.com/user-attachments/assets/dd0a71d7-bf55-43c2-92bc-240842413489)

It wants me to point out a _conf_ file, so there it is :/

**Answer: amportal.conf**

## Task 6

Welp, time to use the exploit:

![image](https://github.com/user-attachments/assets/25ab47c4-488c-457a-a8a5-3f348f25b50c)

Tried it out without making any adjustments to the provided _LFI_, seems like I might've found something useful.

**Credentials found:**

```
User: admin
Pass: jEhdIekWmdjE
```

Apparently it exposes lots of useful information, which is nice.

![image](https://github.com/user-attachments/assets/0c0ba62a-09c7-4101-a78d-174b94ea34a4)

After trying it out on this page, seems like it worked.

Anyways, since port 22 was open, lemme see if the password works for _root_:

![image](https://github.com/user-attachments/assets/25984d24-d871-4ee9-b1ec-dc5c1bb29d0f)

Ok so lemme fix this real quick...

![image](https://github.com/user-attachments/assets/b4dfebce-04f8-4b7b-86bf-54a48d040a4b)

Welp that worked, just had to search up how to add those key algorithms that _ssh_ wanted lmao.

Just in case btw.

**Search Queries:**

```
ssh key exchange method
ssh not matching host key type
```

That way, I can polish up my searching skills (I'm that bad :/).

Luckily, the password also worked, and since we're _root_ now, let's mess around.

Oh and before I forget this time...

**Answer: -oKexAlgorithms=+diffie-hellman-group-exchange-sha1**

**P.S.** Ignore the fact that '+' is missing in my actual usage lmao, apparently **HTB** is very particular in how you set the flag.

## User Flag

![image](https://github.com/user-attachments/assets/20fa2c5b-a95c-49d2-8ca9-3fc894238fad)

**Answer: 2739c5282495bff52dea8426787f92c2**

## Root Flag

Just use the image above lmao.

**Answer: 57511767180a4596cbb05da8d72f3b9d**

## Beyond Root

**There are many other ways to root Beep. These questions after the root flag are hints to help identify them. What is the 2012 CVE ID for pre-authentication remote code execution vulnerablity in FreePBX / Elastix?**

So for this, I initially used _searchsploit_ to see which exploits are listed:

![image](https://github.com/user-attachments/assets/8767a36d-fe3d-43ab-bf96-20ab68894f51)

After finding that, I used _ExploitDB_ to find the _CVE_ id:

![image](https://github.com/user-attachments/assets/219f3b12-e29b-4616-af4f-ebc48f180e12)

Yeah that's pretty much it.

**Answer: CVE-2012-4869**

Anyways, I'm stopping here for now, there's still 3 other questions but I'd rather do another machine first lmao.
