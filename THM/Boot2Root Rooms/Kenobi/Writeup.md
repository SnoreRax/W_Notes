# Tasks

1. [**Deploy the Vulnerable Machine**](#deploy-the-vulnerable-machine)
2. [**Enumerating Samba for shares**](#enumerating-samba-for-shares)
3. [**Gain initial access with ProFtpd**](#gain-initial-access-with-proftpd)
4. [**Privilege Escalation with Path Variable Manipulation**](#privilege-escalation-with-path-variable-escalation)

# Deploy the Vulnerable Machine

## Problems and Answers

![image](https://github.com/user-attachments/assets/ecb98b1e-b13d-489c-beba-ae093dcbd437)

## Solution

![image](https://github.com/user-attachments/assets/2427a30d-aac6-4274-92ad-14a56869ef4f)

**Command:**

```
nmap -sCV [IP Address]
```

**Note:** sudo was not needed lmao, idk why I did that

# Enumerating Samba for shares

## Problems and Answers

![image](https://github.com/user-attachments/assets/fd920b59-c55f-451a-bada-b562a8967fba)

![image](https://github.com/user-attachments/assets/6898c5a9-8931-4798-a659-06911154215e)

![image](https://github.com/user-attachments/assets/70532f5c-9e09-4a56-8c00-a080fb7c0b2c)

## Solution

**Command:**

```
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse [IP Address]
```

Use that to enumerate _Samba shares_

Also here, nice tidbit of info:

![image](https://github.com/user-attachments/assets/48a5533f-0b30-4deb-b226-fdc72c00fa43)

Anyways, back to it:

![image](https://github.com/user-attachments/assets/5f602931-528a-46fb-9b4b-3bf2da99271d)

Enumerate **rpcbind** mount:

![image](https://github.com/user-attachments/assets/b506c955-d437-4588-b22e-1ab4882d7d9b)

**Command:**

```
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount [IP Address]
```

# Gain initial access with ProFtpd

## Problems and Answers

![image](https://github.com/user-attachments/assets/97a68d75-f085-4a36-8c8a-57d4101a3934)

![image](https://github.com/user-attachments/assets/6638aeba-8b09-4e44-ac40-f4bff0a8af52)

![image](https://github.com/user-attachments/assets/c3ca68df-57e7-4300-91a3-499c31cef792)

## Solution

Use this to find **Proftpd** version:

```
nc [IP Address] 21
```

Using **searchsploit** to find the exploits with this version of **Proftpd**:

![image](https://github.com/user-attachments/assets/ea7f442f-7195-41c6-975d-33d4e33dd4e1)

**Command:**

```
searchsploit proftpd [version number]
```

Copying the _id_rsa_ file to _/var/tmp_:

![image](https://github.com/user-attachments/assets/9ffc3ba1-79c4-465e-9dc2-8b38c5e13d89)

Making the _mount_ on my machine (to copy the id_rsa file):

![image](https://github.com/user-attachments/assets/4c4a8295-b045-4fc5-8c6b-62590374b609)

Putting the _mount_ on my machine:

![image](https://github.com/user-attachments/assets/0a7f039b-2023-4d40-8d70-8146aebed590)

Using the _id_rsa_ file to gain access:

![image](https://github.com/user-attachments/assets/6d3e1b78-1e3e-4467-94d2-bbe2e5c91203)

Grab _user.txt_:

![image](https://github.com/user-attachments/assets/d477dd39-1cb2-40cb-8f64-b4f70671755d)

# Privilege Escalation with Path Variable Manipulation

## Problems and Answers

![image](https://github.com/user-attachments/assets/58c90feb-87b5-4071-8dc2-d26525a51c1e)

![image](https://github.com/user-attachments/assets/02e4fe5a-b608-4f36-9f39-5640b05a2f62)

## Solution

![image](https://github.com/user-attachments/assets/5cd233db-71f1-44a0-854f-1b069a0a648d)

![image](https://github.com/user-attachments/assets/f350594e-db1c-4213-a644-eb7b03bb523c)

![image](https://github.com/user-attachments/assets/cb9c827b-165a-4cd5-a371-2354fa1a7688)

**Note:** For the next step, I'm not entirely sure why I can't echo the _/bin/sh_ path to **curl** on _/usr/bin_ lmao

![image](https://github.com/user-attachments/assets/9e4e75c9-dfe9-4f87-b141-49bb0904d48b)
