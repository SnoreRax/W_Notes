# Tasks

[**1. How many open TCP ports are listening on Chemistry?**](#task-1)

[**2. What is the path of the example CIF file available in the dashboard?**](#task-2)

[**3. What is the 2024 CVE ID for a vulnerability related to parsing CIF files using a Python library?**](#task-3)

[**4. What user the CIF Analyzer web application running as?**](#task-4)

# Tools Used

- nmap
- 

# Skills Used

-

# Solutions

## Task 1

![image](https://github.com/user-attachments/assets/586261f3-4430-4d7d-b173-8fda992f7b10)

**Answer: 2**

## Task 2

Let's register first.

![image](https://github.com/user-attachments/assets/9ec9fa76-e56e-47f1-918e-13014f7c1e89)

Anyways, we're greeted with this file upload page here for uploading _cif_ files:

![image](https://github.com/user-attachments/assets/2b739ce9-076c-4b49-9bbb-95d5de2df75f)

We can click the hyperlink there to get an example, and that's the path we wanna find, so let's check _Network_ on the web inspector:

![image](https://github.com/user-attachments/assets/e312e315-e68a-4450-a17a-cb171ec3e8f5)

There it is.

**Answer: /static/example.cif**

## Task 3

Time to check [_ExploitDB_](https://www.exploit-db.com/).

Btw I did but it didn't pop up there, so I just searched '2024 CIF exploitation CVE' and I found it.

![image](https://github.com/user-attachments/assets/7c8254ed-54a8-469a-bbeb-b0abb08d6b48)

**Answer: CVE-2024-23346**

## Task 4

Used writeup for this cuz I got confused lol.
