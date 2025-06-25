# Sudo Strats

## Strat 1

If you use ```sudo -l```, and find that you can run any command but as another user, do this:

```sudo -u [other user] bash -i or /bin/bash```

That way, you get a session as that user instead.
