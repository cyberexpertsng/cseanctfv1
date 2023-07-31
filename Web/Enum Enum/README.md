#### Enum Enum 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/2ab15b0b-c8fa-4bef-bf9b-90ad604ae42f)

Going over to the web url shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/22013b2f-95d7-4d58-88ec-5a70fbfbf007)

Since the challenge name is `Enum` that means we are to enumerate

We can use ffuf to fuzz for `POST` or `GET` request

Doing that got me to `/api` 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6e64b81c-73b6-4abf-8886-dca4c94d1e95)

```
ffuf -c -u https://csean-enum-pain.chals.io/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt -mc all -X POST -fl 11
```

But when I tried fuzzing more values there it just doesn't work

It really frustrated me

Then I decided to use [feroxbuster](https://github.com/epi052/feroxbuster) 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/053c27b2-c187-48ec-ac23-c776bc40c5e2)

```
feroxbuster --url https://csean-enum-pain.chals.io/api/ 
```

Ferobuster got `/api/secret` with `GET` http method

I then accessed it and got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/4887099e-eb07-4c3b-94f9-f64de5998694)

Hmmmm! I then tried using `POST` request to access `/api/secret` and it got me the flag
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/e4f01421-0269-4fd6-94ba-e827553972d3)

```
Flag: csean-ctf{Y0u_SAW_it_in_4_d!fferent_MeTH0D!!}
```
