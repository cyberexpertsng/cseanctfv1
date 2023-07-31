#### Communication Is Key
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/5e2a7a4f-40a9-44c6-b0c2-48a00ebf1d39)

After downloading the attached file checking the file type shows that it is a windows executable
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/38f3d7e1-a260-4a34-bcd6-713f82f383b4)

I normally would try decompile it in ghidra but I don't like decompilling .exe file in ghidra 

So instead what I did was to run it

Doing that I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/0e868522-6cf0-45ed-9bbc-c025f7587896)

From the challenge name `communication` it is likely making some sort of requests 

So confirm that I opened wireshark then listened on all network interface
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/5ddbf73a-26a9-4def-8b27-755ea49f2e1d)

Then I ran the binary again and got this 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/b51361d1-2c72-4296-87f0-d789b6b1a25a)

There are http packets

I followed tcp stream and got the flag
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6a7f6316-899c-4db6-8296-31b503929c35)

Also this binary is a python compiled binary

We can either confirm this by decompilling it or from the user agent we can see it's python2.8

Anyways since we got the flag what's the use of going through that

```
Flag: csean-ctf{CommunicationIsKey_NO_DOUBts!}
```
