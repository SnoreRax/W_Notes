# Tasks

[**1. How many TCP ports are open on Knife?**](#task-1)

[**2. What version of PHP is running on the webserver?**](#task-2)

[**3. What HTTP request header can be added to get code execution in this version of PHP?**](#task-3)

[**4. What user is the web server running as?**](#task-4)

[**5. User Flag**](#user-flag)

[**6. What is the full path to the binary on this machine that james can run as root?**](#task-6)

[**7. Root Flag**](#root-flag)

# Tools Used

- nmap
- curl
- gtfobins

# Skills Used

- Using cURL to send requests
- Finding and using known scripts for exploitation
- Abusing misconfigurations

# Solutions

## Task 1

You know the drill.

![image](https://github.com/user-attachments/assets/35d001eb-418a-467d-8a3e-ea3f729b3dcd)

**Answer: 2**

## Task 2

Let's use ```curl``` for this, shall we?

![image](https://github.com/user-attachments/assets/b3074915-e64a-49d6-bf8c-cf9d0c1308aa)

**Answer: 8.1.0-dev**

## Task 3

Searched it, here's what I found:

![image](https://github.com/user-attachments/assets/639b86e0-c6e3-4cd0-996a-9cc161deb787)

**Answer: User-Agentt**

## Task 4

That _github repo_ included the script for the attack, so just grab it and run it.

![image](https://github.com/user-attachments/assets/7d33e9f8-bed1-4bfe-bd63-5143ba501118)

**Answer: james**

## User Flag

![image](https://github.com/user-attachments/assets/085c0335-27a0-44af-b6d1-d6db76799e69)

**Answer: 25f7e33ed6c21cfafb7e0e6cfa1f88a9**

## Task 6

Use ```sudo -l```:

![image](https://github.com/user-attachments/assets/fc3403d5-6ca5-4dc0-bf50-c6d4b5dbba40)

**Answer: /usr/bin/knife**

## Root Flag

Considering it was a bin, you know what that means B)

![image](https://github.com/user-attachments/assets/4104480e-261d-4f21-a9ca-2ba2813f3c89)

Run that, and the flag is yours.

![image](https://github.com/user-attachments/assets/7deb49c0-684f-4ae0-914d-662c2f7543dc)

**Answer: 513c391edc773ef4819875d5d1e1d4ee**

**P.S.** Here's a bit of a flex to give yourself an ego-boost lol.

**Time Completed In: 14:55**

<img src="https://github.com/user-attachments/assets/110995da-1b92-4df5-8667-7d13dbbe54bf" width="200" height="300">
