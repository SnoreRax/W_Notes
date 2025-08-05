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

[**11. What is the polkit version on the remote host?**](#task-11)

[**12. What is the 2021 CVE ID for the vulnerability in this version of polkit related to bypassing credential checks for D-Bus requests?**](#task-12)

[**13. Root Flag**](#root-flag)

# Tools Used

- nmap
- curl
- linpeas

# Skills Used

- CVE exploitation
- AI prompt injection

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

## Task 11

This task stumped me, so I looked at a writeup. I forgot _linpeas_ was a thing lol. So I used that.

<img width="722" height="714" alt="image" src="https://github.com/user-attachments/assets/7967adc8-8701-4137-94e9-bbb25f0064bc" />

I'll check if it found what the task is looking for.

<img width="229" height="33" alt="image" src="https://github.com/user-attachments/assets/3d808ab9-b9a4-477a-b283-157514d0be5c" />

This part is interesting, cuz after I searched it, I found this:

<img width="706" height="644" alt="image" src="https://github.com/user-attachments/assets/d9252933-579b-45e5-8804-671dc2e59dc8" />

At least I know what it's leading up to now, but first, I still have to find the specific **polkit** version that's on this machine.

<img width="358" height="83" alt="image" src="https://github.com/user-attachments/assets/db85761a-3540-42f2-88f1-b3fa369d52b0" />

I gave up on trying to read through the _linpeas_ report lmao. I just searched for the command to check the **polkit** version.

**Answer: 0.115-6**

## Task 12

I already found that earlier lol.

**Answer: CVE-2021-3560**

## Root Flag

To exploit the **CVE** found, follow this [**blog**](https://github.blog/security/vulnerability-research/privilege-escalation-polkit-root-on-linux-with-bug/), it provides detailed instructions on how the exploit works.

Interestingly enough, it involves some timing lol. You'll know once you read it. 

Because of that timing, I instead resorted to trying this [**PoC**](https://github.com/secnigma/CVE-2021-3560-Polkit-Privilege-Esclation/blob/main/poc.sh) because I couldn't get the timing right lol. Every time I try to set the password for the new user made, it would result in not being able to detect the PID of the command issued and erasing the user I made beforehand, forcing me to reset the process each time. It's not efficient that way, so hopefully the script works.

<img width="687" height="833" alt="image" src="https://github.com/user-attachments/assets/0a5452a7-e6e4-4fb3-bf82-a523b4e71f50" />

Anyways, if you notice here, it took a few tries. The reason is because it's a timing attack, it's to be expected that the first go may not work right off that bat. Keep that in mind next time you do time-based attacks. 

<img width="682" height="568" alt="image" src="https://github.com/user-attachments/assets/99ca6e23-d720-40e0-acda-b9cea6d85f84" />

Interestingly enough, right after I wrote that down, I lost my user and had to run the attack again. Time-based attacks really are volatile lol.

**Answer: 129b415dd8ee4cd5adf3cfd94138c954**
