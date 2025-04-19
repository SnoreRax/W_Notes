# Tasks

1. [Reconnaissance](#reconnaissance)
2. [Getting a shell](#getting-a-shell)
3. [Privilege Escalation](#privilege-escalation)

## Reconnaissance

### Problems

![image](https://github.com/user-attachments/assets/9de237a6-a043-4d38-b70f-364572439cc8)

### Results

**Nmap Scan:**

![image](https://github.com/user-attachments/assets/87ff6469-9370-4d15-a48a-bcd3a4b116eb)

**Gobuster Scan:**

![image](https://github.com/user-attachments/assets/7e07078b-3272-4fd8-8709-8ddc8d6d5c9d)

### Answers

1. **2**
2. **2.4.29**
3. **ssh**
4. N/A
5. **/panel/**

## Getting a shell

### Problem

![image](https://github.com/user-attachments/assets/b7552d78-29c1-45d9-9091-e0f9a1805846)

### Results

![image](https://github.com/user-attachments/assets/2a24ece1-2773-4af7-a237-0532785ad049)

**Note:** Used this for the PHP rev shell script: [Online Reverse Shell Generator](https://www.revshells.com/)

**Note 2:** Just to clarify, only the _.php_ file extension is not allowed, but the other _.php_ extensions are allowed. 

Source: **BurpSuite Intruder**(Sniper Payload on _.php_ testing all the other extensions)

![image](https://github.com/user-attachments/assets/ab47728a-a106-4374-b1f2-ab407b163005)

![image](https://github.com/user-attachments/assets/1a0545aa-71c9-4417-b2b9-94d1e1168972)

### Answer

1. **THM{y0u_g0t_a_sh3ll}**

## Privilege escalation

### Problems

![image](https://github.com/user-attachments/assets/b8552afe-ffe2-4b75-bea6-e4041130de91)

### Results

![image](https://github.com/user-attachments/assets/e9d93996-df5f-45d5-9da1-643825eeebeb)

**Use this:** [GTFO Bins](https://gtfobins.github.io/)

![image](https://github.com/user-attachments/assets/1d79a07d-8590-4c3a-8d89-f8df2fa95312)

### Answers

1. **/usr/bin/python**
2. N/A
3. **THM{pr1v113g3_3sc4l4t10n}**
