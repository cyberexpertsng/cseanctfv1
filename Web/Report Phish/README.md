#### Report Phish
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/b00464a0-4a7e-4cbb-88f6-ce53d0907e93)

Going over to the url shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/18efa1ef-fa76-474a-ad2f-a29b620ca23b)

Seems to be a service used for reporting sites used for phishing

To check if it indeeds make some sort of http request to the site submitted I used [webhook.site](https://webhook.site/) 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/56cc7657-b8bc-4ce0-9804-63594bc853b1)

Submitting that url I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/21f1c96b-293a-44cf-855d-43cff1e343b3)

Then after some minutes of waiting patiently (I didn't wait patiently I sent the request multiple times 😂) I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/414a6d32-090d-4c51-a6c7-8cbe03fce0d5)

The flag is in the referer header

```
Flag: csean-ctf{TH!S_really_l00ks_l!ke_A_PHISH_OR_NOT?} 
```
