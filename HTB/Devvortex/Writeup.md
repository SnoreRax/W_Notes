# Tasks

[**1. How many open TCP ports are listening on Devvortex?**](#task-1)

[**2. What subdomain is configured on the target's web server?**](#task-2)

[**3. What Content Management System (CMS) is running on dev.devvortex.htb?**](#task-3)

[**4. Which version of Joomla is running on the target system?**](#task-4)

# Tools Used

- nmap
- ffuf
- 

# Skills Used

-

# Solutions

## Task 1

<img width="760" height="353" alt="image" src="https://github.com/user-attachments/assets/ff3d1a0f-609b-4f54-96d3-a4d2a24309d5" />

**Answer: 2**

## Task 2

Here's the _ffuf_ scan for **subdomain enumeration**:

<img width="941" height="496" alt="image" src="https://github.com/user-attachments/assets/fc01a2ed-84a4-4640-a8d5-6b6f555fab42" />

**Answer: dev.devvortex.htb**

## Task 3

So after running _ffuf_ again, but this time for **directory fuzzing**, here's what I found:

<img width="754" height="544" alt="image" src="https://github.com/user-attachments/assets/c9dd23c8-a140-4259-b5b1-bc710f7fda40" />

That ```robots.txt``` directory looks nice, so I checked that too.

<img width="494" height="501" alt="image" src="https://github.com/user-attachments/assets/18562752-f8c4-4908-8d55-f4ac6c201196" />

And there it is.

**Answer: joomla**

## Task 4

