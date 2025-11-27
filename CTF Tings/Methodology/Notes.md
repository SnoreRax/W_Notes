# Yes

Rough draft for now just to put my thoughts down on how to solve challenges, for mainly these two categories:

[**Web**](#web)

[**Pwn**](#pwn)

On a general note, don't be afraid to use ChatGPT to learn. For solving challenges on the other hand, ur a pussy if u use it too much >:)

## Web

1. Remember that the website is always gonna be hosted on `/var/www/html` in the file directory of the server it's hosted on
2. Always try going to `/robots.txt` just to see if it's there. Who knows, might find a nice endpoint
3. If the website has a lot of endpoints, explore it normally first. If not, you can still explore it normally but if anything suspicious comes up use `Inspect Source`
4. If you see a function in the source code (particularly in _.js files_) that might be of use, try running it in console to still have access to local variables
5. Use `Burpsuite` or `Network Tool` to see how data is sent/received, it's usually in _JSON_ format
6. For `SSTI` challenges, if you're absolutely certain that Python is being used, and the usual _SSTI payloads_ aren't working, try using simple _Python functions_ like `open('/flag.txt).read()` in place of the
actual payload, so it'll look like this: `${open('/flag.txt').read()}`
7. Speaking of Python, you should familiarize yourself with some of its functions to read files and execute commands at will

## Pwn

1. Buffer Overflow is NOT that easy
2. Keep spamming `AAAA%p,%p...` in _format strings challs_ until you get **0x41414141** to find out where your payload should start
3. On that same note, just because the payload starts there, doesn't mean that's where you actually execute your attack (add 1 to the position per argument, e.g. format specifiers)
4. For _Buffer Overflow challs_, to make it easier on yourself, whenever you suspect a certain pattern might be the offset and you want to search for it in `gdb` via `pattern offset [pattern]`, add one more byte
to the end of the pattern to account for _null byte_
