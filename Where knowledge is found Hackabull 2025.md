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

We do see that there is encryption involved so lets check the main function.

![image](https://github.com/user-attachments/assets/ba2d129e-0e47-48fc-8b5c-4f4c906a9aee)

This may all seem confusing at first, but there are two ways that we can use to understand better. 

