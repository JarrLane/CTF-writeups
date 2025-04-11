This challenge was part of the archived challenges of imaginary CTF. Imaginary CTF often hosts daily ctf challenges and it is a great way to practice CTF. Even though this challenge was archived, I still decided to complete it and do a writeup on it.

Challenge (Misc category): 
![image](https://github.com/user-attachments/assets/afe12561-9517-418e-aff8-c2ff9c3d6f24)

Because this is an archived challenge, we won't be able to connect, so I will just do the exploit locally.

Looking at the code, there is a flag variable that contains the flag and an eval function that takes in input from us that is up to 8 characters max ([:8] means that out input will be an array that holds 8 values from index 0-7, the rest of the input is ignored, if you are curious about this look up slicing in python)

The function eval() will evaluate anything that is input into it, this is very dangerous because an attacker can input something malicious that gets evaluated and executed and this can have a range of consequences. It is strongly reccomended to avoid using eval() with user input because of how detrimental it can be. 

Because of how eval() can be exploited, there is a category of CTF called "jails". These challenges intentionally use eval(), but hinder how you can interact with it, which makes it up to you to find how to bypass the obstacles/ break free from the limitations in order to make the eval() function do what you want. In this case, we are limited to an 8 character input, and we want to try to read the flag.

There are many ways to leak the name of a variable, but in our case, we are limited to 8 characters. One possible way to leak flag is by using the int() function, this function converts a value to an int. If we try to pass text into this function it gives an error: 
![image](https://github.com/user-attachments/assets/04958e68-521c-486d-a392-cb0af311318a)

![image](https://github.com/user-attachments/assets/055b9455-2861-43a2-9f34-507b261a48a1)

In the error, it tells us the value of test, so now we need to apply this to the flag

What happens if we try int(flag)? It wont work because that is 9 characters and we can only have 8.

Is there a way to make this work with 8 characters? Yes

The key is in how python handles UTF encoding with identifiers/variable names. 

UTF is for representing unicode characters. Unicode characters are a standard that tries to digitize and assign a code/ value for the diverse alphabets and writing systems used by the world. Because there are many diverse characters and alphabets, some letters may seem the exact same, for example: here is a and here is а, they may look the same but one of them is from the latin alphabet and the other is from the cyrilic, if you were to encode those two letters you would get two different values. Unicode also has different formats that may reprewsent the same letter but have different unicode values to distinguish them, this can be a problem for a computer.

The solution is normalization. Normalization (without getting too deep) is the process of trying to simplify different unicode characters to a more standardized form. For example, ℜ gets normalized to R. There are four types of normalization, but we care about NFKC. 

Before parsing variables, python will use NFKC normalization on the identifiers, so we want to know if maybe there is a unicode character that will somehow get treated as 2 characters, which would allow us to pass in 9 characters from 8 characters. It turns out there is. 

Here is that wierd character: ﬂ (if you are curious about it check out https://www.thoughtco.com/ligature-in-typography-1078102)

Because of how NFKC works, int(ﬂag) will be parsed as int(flag), which will evaluate and meet the 8 character limit, lets try it:

![image](https://github.com/user-attachments/assets/343fc808-1172-4d42-9dcd-e68f246d67d8)

In the value error we leaked the flag

This challenge was very interesting and taught me more about python, thank you for reading my writeup.



