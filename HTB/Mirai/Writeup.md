# Tasks

[**1. What is the name of the service running on TCP port 53 on Mirai? Don't include a version number.**](#task-1)

[**2. What unusual HTTP header is included in the response when visiting the service on port 80?**](#task-2)

[**3. What relative path on the webserver presents the Pi-hole dashboard?**](#task-3)

[**4. What was the default username on a Raspberry Pi device?**](#task-4)

[**5. What is the default password for the pi user?**](#task-5)

[**6. User Flag**](#user-flag)

[**7. Can the pi user run any command as root on Mirai?**](#task-7)

[**8. The flag-less root.txt file mentions that it's on the USB stick. What is the mountpoint for a device that is labeled as a USB stick on this host?**](#task-8)

[**9. What is the full path to the device that represents the raw USB media on Mirai?**](#task-9)

[**10. When files are deleted from a drive, is the memory definitely immediately overwritten with something else?**](#task-10)

[**11. Root Flag**](#root-flag)

# Tools Used

- nmap
- curl
- gobuster

# Skills Used

- Sending requests via cURL
- Directory fuzzing
- A wee bit of OSINT
- File recovery (kind of)

# Solutions

## Task 1

Usual scan:

![image](https://github.com/user-attachments/assets/8c361038-ef9c-4b22-b898-822bbf3ed33a)

**Answer: dnsmasq**

## Task 2

Use this:

```
curl -i http://[IP Address]
```

It doesn't have a domain name so yeah.

![image](https://github.com/user-attachments/assets/bcb05428-612c-4b9f-88c4-317dc3c23d07)

Anyways, that header is pretty weird huh.

**Answer: X-Pi-hole**

## Task 3

Time for a _gobuster_ scan baby:

![image](https://github.com/user-attachments/assets/6f90545d-aee8-4de2-9431-55272f295a0e)

Yeah that's right, I didn't finish the scan lmao. Although tbf, _/admin_ was interesting enough to skip for anyway, which apparently worked.

![image](https://github.com/user-attachments/assets/1f53ecfc-8c94-43cf-945a-51e41bae2562)

**Answer: /admin**

## Task 4

Just search this:

```
raspberry pi default username
```

![image](https://github.com/user-attachments/assets/42ee26ce-61bd-4591-b161-e0c365ecdfde)

Welp, there's our answer.

**Answer: pi**

## Task 5

Same result as above lmao.

**Answer: raspberry**

## User Flag

![image](https://github.com/user-attachments/assets/d8d1a374-7e36-4537-a1f8-d8a7e2355772)

**Answer: ff837707441b257a20e32199d7c8838d**

## Task 7

![image](https://github.com/user-attachments/assets/16ae7349-0f4a-430f-aba4-53bddfc33950)

**Yes you can.**

**Answer: yes**

## Task 8

![image](https://github.com/user-attachments/assets/d893ad7c-dda9-402c-b217-4436304a160f)

Damn they rly did lose it :/

Anyways, let's go search _/mnt_, pretty sure it's there.

![image](https://github.com/user-attachments/assets/102849e0-1ee7-4828-bab6-9d2418dd83a4)

Apparently not :/

Aight time to check _/media_.

![image](https://github.com/user-attachments/assets/00590a97-c936-4095-abd5-c26a4d989180)

Oh.

**Answer: /media/usbstick**

## Task 9

![image](https://github.com/user-attachments/assets/844ae668-fbad-4404-929d-5e412f06cbfe)

Ok now that ain't good :/

Anyways, ig it's time to search how to find deleted USB files on Linux file systems. Or ar least I would, if I hadn't done this:

```
sudo find / -name root.txt -type f 2>/dev/null
```

Such a cheatsy way to do it yeah XD

Too bad that's not what we're looking for though in this task, so instead, we do this:

```
mount | grep usb
```

![image](https://github.com/user-attachments/assets/7c392687-9e26-426d-b65b-3181367d9894)

**Answer: /dev/sdb**

## Task 10

Yeah I'm just gonna search that lmao.

![image](https://github.com/user-attachments/assets/1f12042e-5c07-4e7a-af67-59f00049bf9c)

Well ig that's our answer lmao.

**Answer: no**

## Root Flag

Ok so apparently the flag wasn't in the ```find``` command I did earlier lmao, it's in _/dev/sdb_, so:

![image](https://github.com/user-attachments/assets/a56ce11d-353c-4c32-b664-3741e2095b74)

![image](https://github.com/user-attachments/assets/47c0d7ff-14a2-40f9-8d3a-dd46f78fc2b9)

Yeah whatever if it works, it works lmao.

**Answer: 3d3e483143ff12ec505d026fa13e020b**
