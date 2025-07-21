# Tasks

[**1. How many open TCP ports are listening on Dog?**](#task-1)

[**2. What is the name of the directory on the root of the webserver that leaks the full source code of the application?**](#task-2)

[**3. What is the CMS used to make the website on Dog? Include a space between two words.**](#task-3)

[**4. What is the password the application uses to connect to the database?**](#task-4)

[**5. What user uses the DB password to log into the admin functionality of Backdrop CMS?**](#task-5)

[**6. What system user is the Backdrop CMS instance running as on Dog?**](#task-6)

# Tools Used

- nmap
- 

# Skills Used

-

# Solutions

## Task 1

<img width="765" height="533" alt="image" src="https://github.com/user-attachments/assets/3560138d-014f-4ee4-be4b-d8050212c8b3" />

Just the usual.

**Answer: 2**

## Task 2

So I was gonna use _ffuf_ for directory brute-forcing, but I just noticed that the _nmap_ scan actually included a few interesting directories. Specifically **robots.txt** to view the full details.

<img width="515" height="752" alt="image" src="https://github.com/user-attachments/assets/df47d34b-a337-4243-959a-31a3fc8029b3" />

**Spoiler Alert:** That wasn't it lmao. I didn't notice, but **.git** was found in the _nmap_ scan.

<img width="757" height="524" alt="image" src="https://github.com/user-attachments/assets/74322ddf-82ca-43ea-8ea8-4c2a87f17d6b" />

**Answer: .git**

## Task 3

<img width="250" height="136" alt="image" src="https://github.com/user-attachments/assets/a386d8aa-8d0a-42f8-bdd0-653b87343e45" />

It's just at the bottom of the website lmao.

**Answer: Backdrop CMS**

## Task 4

Used the hint cuz I got lost lol. Apparently I was supposed to use ```git-dumper``` got grab the **.git** of the website.

**How to use:**

```
git-dumper [URL] [DIR]
```

**Example:**

```
git-dumper http://10.10.11.58/.git/ ~/.git
```

Directory's there just ~~in case the **.git** directory is nested for some reason~~ ok so that was a lie, apparently it was to specify where the **.git** repo would be downloaded to and I just learned that the hard way :/

Anyways, now what I fixed that.

<img width="456" height="303" alt="image" src="https://github.com/user-attachments/assets/a8226fd0-01fe-4f32-a14f-bc0950227750" />

Let's go look for that password.

<img width="778" height="410" alt="image" src="https://github.com/user-attachments/assets/e0f89338-a38b-4d36-9900-94c2c741de28" />

Found it here, could be useful too since it's root.

**Answer: BackDropJ2024DS2024**

## Task 5

Ok so apparently it wasn't **root** lmao. Still gotta find the username. There are users listed in the home page, but none of them worked. Then I went to the 'About' Page, and found the email format. Since I have the **.git** repo, I could use this to do the job:

```
grep -R '@dog.htb' *
```

<img width="803" height="72" alt="image" src="https://github.com/user-attachments/assets/d17d062a-9906-457d-a3c7-46eb22461117" />

All I found was **tiffany** lmao, let's try that.

<img width="1920" height="696" alt="image" src="https://github.com/user-attachments/assets/d8cf235f-9763-4db0-8437-af5513ebefff" />

That worked.

**Answer: tiffany**

## Task 6

Ok ig it's time to exploit, let's see what we're working with.

