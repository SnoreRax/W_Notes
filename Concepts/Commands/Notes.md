# Checking for files with SUID bit set

```
find / -user root -perm -4000 2>/dev/null
```

**Note:** There are other ways of using this command, this the only one I'm most comfy with.

Also, if you need to (since I usually do this to find vulnerable binaries), you can narrow it down to this:

```
find /usr/bin -user root -perm -4000 2>/dev/null
```

# Privilege Escalation

## Sudo Strats

If you use ```sudo -l```, and find that you can run any command but as another user, do this:

```sudo -u [other user] bash -i or /bin/bash```

That way, you get a session as that user instead.
