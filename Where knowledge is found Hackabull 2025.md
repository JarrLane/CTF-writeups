Category: Reversing

This challenge was from an in person event at USF (University of South Florida) called Hackabull, it was a hackathon and a CTF. 
I learned alot from this event and our team won 3rd place. This was a great event and a good weekend overall.

![image](https://github.com/user-attachments/assets/16a06c4e-e6a7-4c16-843a-0d4e48c50682)


In this challenge we are given a binary executable (compiled code/machine code) and we are told there is a type of encryption that 
needs to be broken. The first thing to do is look at the type of file being dealt with. This is very important because it tells us
things such as little/ big endian and more. When I check, I see its an ELF (executable linux file) 64-bit LSB shared object
 x86-64.

 Now to reverse engineer, I will use binary ninja to examine the binary. In reverse challenges, it is common practice to check the strings present in the binary, in some cases the flag will be there bit not in this case.

 ![image](https://github.com/user-attachments/assets/2731fe5d-808f-4144-8b07-bafed488ca3a)

We do see that there is encryption involved so lets check the main function. In the settings I will have it use Pseudo C, which is reconstructed C code from the binary that the decompiler generates. (Using pseudo C is much easier than trying to read assembly or machine code, especially if you have C experience)

![image](https://github.com/user-attachments/assets/a5005edc-2f1d-4216-b28f-d4075e148cb3)
![image](https://github.com/user-attachments/assets/cfb9ad5f-26f3-4e97-999a-44a3cd129606)


This may all seem confusing at first, but there are two ways that we can use to understand better.  

The first way is having a good understanding of C, assembly, IL, etc. If you are already familiar with these languages you can probably piece together what the code is doing. There is also a feature in binary ninja that lets you rename variables from the names 
given by pseudo C for better readability. 

The second way is to use chat GPT. In general I have mixed feelings about using AI in CTF challenges, but I think its valid to use it to try to understand code, especially if you are not familiar with C or coding in general. If you give chat GPT the pseudo C it can run you through how it works so you get a better idea. If you use chat GPT make sure you learn from using it.

In my case I use both. I can get by when it comes to reading C, but I like to use chat GPT to check to make sure Im on the right track.

Since I want this writeup to be beginner friendly, I am going to walk you through the code.



