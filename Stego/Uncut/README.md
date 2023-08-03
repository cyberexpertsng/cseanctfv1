Looking at the image given in the question below; 

![Alt Text](https://raw.githubusercontent.com/cyberexpertsng/cseanctfv1/main/Stego/Uncut/2023-07-12_15-23.png)

The question is talking about Uncut and after downloading the full image we have it as shown below.
![Alt Text](https://raw.githubusercontent.com/cyberexpertsng/cseanctfv1/main/Stego/Uncut/uncut%20-%20helping%20family%20with%20cybersecurity.png)

Looking at the PNG image provided and the combination of words within the question; should give participants the hint to find this flag. Considering the word uncut and carefully looking at the image; one can see the family is watching something on the TV and it look blurred but that was a rabbit hole. The focus is uncut and the words within the question say something CSEAN do to help the family.

Investigating the picture reveals some modification has taken place as the PNG extension reveals that we can modify other IDAF properties. Using a tool named TweakPNG, manipulating the image size reveal the missing part of the image which contains the flag as shown below.

![](https://raw.githubusercontent.com/cyberexpertsng/cseanctfv1/main/Stego/Uncut/edit.png)

![](https://raw.githubusercontent.com/cyberexpertsng/cseanctfv1/main/Stego/Uncut/uncut%20-%20helping%20family%20with%20cybersecurity%202.png)

```
Flag: csean-ctf{csean-ctf{CSEAN-STOP-THINK-CONNECT}
```
