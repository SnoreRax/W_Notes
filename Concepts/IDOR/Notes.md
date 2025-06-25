# IDOR

## Simple version

If you find that in the URL, it says something along this line:

```
http://blahblah.com/userid=1
```

Try to see if you can change that ```userid``` to any other value and change user cause of that.

## Cookie version

In _Storage_ when checking _developer tools_, check to see if you can find the cookie of the user you're logged in as.

After that, check to see if you can decrypt it to see how it stores credentials, and see if you can make your own to log in as _admin_.

Here's an example:

**JWT**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30
```

Use [**jwt.io**](https://jwt.io/) to decode that, then you'll be able to see credentials.
