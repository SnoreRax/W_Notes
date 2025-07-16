# Tasks

[**1. What is the name of the webserver running on port 80 and 443 according to nmap?**](#task-1)

[**2. What is the name of the application that presents a login screen on port 443?**](#task-2)

[**3. What txt file can be found on the webserver that contains user information?**](#task-3)

[**4. What is the username found in the system-users.txt file?**](#task-4)

[**5. What is the default password for a pfsense installation?**](#task-5)

[**6. What version of pfSense is running on Sense?**](#task-6)

[**7. What 2016 CVE ID describes a command injection vulnerability in a PHP page on pfSense via a GET parameter?**](#task-7)

[**8. User Flag**](#user-flag)

[**9. Root Flag**](#root-flag)

# Tools Used

- nmap
- ffuf
- metasploit

# Skills Used

- Directory fuzzing using ffuf and a really big wordlist
- Bit of OSINT
- Exploiting CVEs via Metasploit

# Solutions

## Task 1

You know the drill.

<img width="943" height="392" alt="image" src="https://github.com/user-attachments/assets/d9f184d8-8bb1-4857-ae35-a0bbddd99845" />

**Answer: lighttpd**

## Task 2

This is what I found upon going to the website:

<img width="392" height="384" alt="image" src="https://github.com/user-attachments/assets/f77a750e-8ad3-4d4f-8c3c-be026ec1a4cb" />

At first, I thought it was just **sense**, but when that didn't work, I cut the logo and used reverse image search to find something about it.

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/12da0736-989b-49e8-be01-7230eed18c6f" />

This is what I found. (Useful for later to identify vulnerabilities to this particular software)

**Answer: pfsense**

## Task 3

Time to get _ffuf_, hopefully I don't need to to recursive directory search.

**Command:**

```
ffuf -u https://[Host Address]/FUZZ -w /usr/share/wordlists/dirb/common.txt -ac -e .txt
```

<img width="743" height="505" alt="image" src="https://github.com/user-attachments/assets/4bddd3e3-4e44-462d-93a2-48f17962f10e" />

This was the initial search using the **common.txt** wordlist, but unfortunately, it didn't find it. I'll need a bigger wordlist (used **big.txt** this time, still under _dirb_).

<img width="759" height="477" alt="image" src="https://github.com/user-attachments/assets/81239527-2b89-452c-9a55-5a8da6d5b969" />

Still nothing :/

I'm using another wordlist.

<img width="947" height="446" alt="image" src="https://github.com/user-attachments/assets/63d38c47-f3a1-4b32-9eef-97d653f51fc4" />

Too lazy to type out the name and path of the wordlist I used this time, but let's just say it's really big. I'll come back once it's done.

**4 hours later (I headed out lmao)**

<img width="772" height="313" alt="image" src="https://github.com/user-attachments/assets/5b5f4aa5-6836-469a-ab12-670d4950d0c3" />

<img width="559" height="181" alt="image" src="https://github.com/user-attachments/assets/c8ec87cf-bb7b-4f66-af0c-02c8e1a0100e" />

Finally.

**Answer: system-users.txt**

## Task 4

Just look at the previous task, it's there lol.

**Answer: rohit**

## Task 5

Ok at first I thought it was **company defaults** cuz that was what was on the user.txt file, but apparently it's not.

I'm just gonna search it.

<img width="518" height="261" alt="image" src="https://github.com/user-attachments/assets/a2ebb19c-8b35-47db-83be-a05927a60399" />

There it is.

**Answer: pfsense**

## Task 6

Ok let's login.

<img width="1192" height="849" alt="image" src="https://github.com/user-attachments/assets/0b21ce60-b476-4797-9a15-c733b08422af" />

Here it is. The version number is also right there btw so yeah.

**Answer: 2.1.3-RELEASE**

## Task 7

Searched it, saw mention of _Metasploit_ in one of the sites I visited, so I'll use _searchsploit_ to see if it's available to use there.

<img width="945" height="150" alt="image" src="https://github.com/user-attachments/assets/d75333ef-0650-453f-893a-1aaa859ee1c3" />

Found this but I'm not sure if it's right, since the question mentioned 2016, and upon searching this one was 2014. Guess I'll keep looking.

Ok so this time I used a different search parameter. Instead of the version number outright, I broadened it to _"pfsense vulnerability 2016 cve"_.

<img width="785" height="903" alt="image" src="https://github.com/user-attachments/assets/164374a5-6186-40a5-ad6f-6dec99d1cf7f" />

And found this.

<img width="1102" height="179" alt="image" src="https://github.com/user-attachments/assets/a2cc03ab-ef3f-4a94-81b9-7440db867048" />

There was even this huh. Seems like I can use _Metasploit_ after all.

**Answer: CVE-2016-10709**

## User Flag

Since _Metasploit_ can be used for this, I'm using that lol.

<img width="941" height="503" alt="image" src="https://github.com/user-attachments/assets/f08f3f03-d4f9-462b-b28d-948bf909e0d7" />

Don't forget to set options. Oh, and use the **shell** command on _meterpreter_ to use a regular shell lol. (Personal preference btw)

Btw...

<img width="58" height="42" alt="image" src="https://github.com/user-attachments/assets/f64c86c7-d9e6-4382-8065-40cc6b72ef2e" />

Apparently **Rohit** was **root** lmao. Guess I'm getting both flags immediately.

<img width="271" height="187" alt="image" src="https://github.com/user-attachments/assets/51788311-b41e-49af-a313-a8a4bafb7b6e" />

**Answer: 8721327cc232073b40d27d9c17e7348b**

## Root Flag

<img width="265" height="258" alt="image" src="https://github.com/user-attachments/assets/f0f2c826-7ae6-44f0-bfdc-fb9fa85f953d" />

Yeah.

**Answer: d08c32a5d4f8c8b10e76eb51a69f1a86**
