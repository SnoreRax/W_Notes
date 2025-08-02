# Tasks

[**1. How many TCP ports are open on the remote host?**](#task-1)

[**2. What is the domain for the Wordpress blog?**](#task-2)

[**3. Which 2019 CVE is the wordpress version vulnerable to?**](#task-3)

[**4. What is the secret registration URL of the employee chat system?**](#task-4)

[**5. What is the name of the bot running on the Rocket Chat instance?**](#task-5)

[**6. Which recyclops commands allows listing files?**](#task-6)

[**7. What is the file name of the file that contains the configuration information of hubot running on the chat system?**](#task-7)

[**8. What is the password obtained from that configuration information?**](#task-8)

[**9. Which regular user with a home directory exists on Paper other than rocketchat?**](#task-9)

[**10. User Flag**](#user-flag)

[**11. **](#task-11)

# Tools Used

- nmap
- 

# Skills Used

-

# Solutions

## Task 1

<img width="788" height="571" alt="image" src="https://github.com/user-attachments/assets/a6dfe0de-7747-4a16-9a8e-6fc0251a0192" />

**Answer: 3**

## Task 2

For this part, use ```curl``` to get the HTTP response headers to find the domain name.

<img width="517" height="366" alt="image" src="https://github.com/user-attachments/assets/263e772d-308a-4865-8171-5431818f5518" />

Like that.

**Answer: office.paper**

## Task 3

Now that we have that domain name, put that into our ```/etc/hosts``` file so that we can access it.

<img width="1920" height="849" alt="image" src="https://github.com/user-attachments/assets/e547ddf8-98b1-462e-a936-e24f8c5490af" />

And we're greeted with this. For now though, let's find the **WordPress** version being used here.

<img width="497" height="544" alt="image" src="https://github.com/user-attachments/assets/1dcba3cf-a8b5-482a-8124-8f95c29d9168" />

For once in a long time, _Wappalyzer_ became useful lol.

Now let's search for **CVEs** related to that version.

<img width="1816" height="900" alt="image" src="https://github.com/user-attachments/assets/6f11299c-073e-488b-aa7b-7e886fcabe83" />

Here it is. The contents are pretty simple too.

**Answer: CVE-2019-17671**

## Task 4

Use the **CVE** we just found to view some secrets.

<img width="1920" height="776" alt="image" src="https://github.com/user-attachments/assets/8d450fda-aca7-4823-8160-8a6a4eaea975" />

Like that lol.

**Answer: http://chat.office.paper/register/8qozr226AhkCHZdyY**

## Task 5

<img width="1920" height="770" alt="image" src="https://github.com/user-attachments/assets/486443a6-f1d9-4679-a713-59238bba1e1c" />

Yep.

<img width="1520" height="539" alt="image" src="https://github.com/user-attachments/assets/80a296ef-7fb7-4227-aceb-f0ba1ad832fc" />

Anyways, here's the bot.

**Answer: recyclops**

## Task 6

Using the ```help``` command, I found this:

<img width="856" height="639" alt="image" src="https://github.com/user-attachments/assets/9b83ba50-86cb-45c1-90fd-086b27f3232e" />

Upon using it:

<img width="379" height="225" alt="image" src="https://github.com/user-attachments/assets/dd908b5c-71e8-4d20-a758-64cf68a080d7" />

Yep. It works.

**Answer: list**

## Task 7

Alright, so after some digging around, I noticed that the bot was vulnerable to file path traversal. After doing that, I found this:

<img width="413" height="314" alt="image" src="https://github.com/user-attachments/assets/6df0393f-2daf-42db-b173-56838d715bb8" />

**Answer: .env**

## Task 8

Refer to the screenshot above.

**Answer: Queenofblad3s!23**

## Task 9

Ok so I tried to login as the bot, then this happened.

<img width="538" height="167" alt="image" src="https://github.com/user-attachments/assets/a6fb31ea-4420-4228-9ee5-b8f9f5291d5a" />

I got trolled :/

Anyways, maybe the password was reused though, considering the task details. So let's see who the other user is.

<img width="401" height="212" alt="image" src="https://github.com/user-attachments/assets/e0da87c5-8de1-4ca6-bb0c-b6697a47e37c" />

There it is.

**Answer: dwight**

## User Flag

<img width="1919" height="698" alt="image" src="https://github.com/user-attachments/assets/2bad9117-5d76-4662-93cb-5eae6614f409" />

Ok apparently it's not for that.

I got stumped so I looked at a writeup, I forgot ```port 22``` was open :/

<img width="646" height="227" alt="image" src="https://github.com/user-attachments/assets/8c9852a4-8beb-447c-b0e8-1c2dfa53a771" />

Yep.

**Answer: 023947a5defdd59ce800ef15087797a5**
