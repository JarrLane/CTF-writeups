Category: Reversing

This challenge was from an in person event at USF (University of South Florida) called Hackabull, it was a hackathon and a CTF. 
I learned alot from this event and our team won 3rd place. This was a great event and a good weekend overall.

![image](https://github.com/user-attachments/assets/16a06c4e-e6a7-4c16-843a-0d4e48c50682)


In this challenge we are given a binary executable (compiled code/machine code) and we are told there is a type of encryption that 
needs to be broken. The first thing to do is look at the type of file being dealt with. This is very important because it tells us
things such as little/ big endian and more. When I check, I see its an ELF (executable linux file) 64-bit LSB shared object
 x86-64.

 Now to reverse engineer, I will use binary ninja to examine the binary. In reverse challenges, it is common practice to check the strings present in the binary, in some cases the flag will be there but not in this case.

 ![image](https://github.com/user-attachments/assets/2731fe5d-808f-4144-8b07-bafed488ca3a)

We do see that there is encryption involved so lets check the main function. In the settings I will have it use Pseudo C, which is reconstructed C code from the binary that the decompiler generates. (Using pseudo C is much easier than trying to read assembly or machine code, especially if you have C experience)

![image](https://github.com/user-attachments/assets/a5005edc-2f1d-4216-b28f-d4075e148cb3)
![image](https://github.com/user-attachments/assets/cfb9ad5f-26f3-4e97-999a-44a3cd129606)


This may all seem confusing at first, but there are two ways that we can use to understand better.  

The first way is having a good understanding of C, assembly, IL, etc. If you are already familiar with these languages you can probably piece together what the code is doing. There is also a feature in binary ninja that lets you rename variables from the names 
given by pseudo C for better readability. 

The second way is to use chat GPT. In general I have mixed feelings about using AI in CTF challenges, but I think its valid to use it to try to understand code, especially if you are not familiar with C or coding in general. If you give chat GPT the pseudo C it can run you through how it works so you get a better idea. If you use chat GPT make sure you learn from using it.

In my case I use both. I can get by when it comes to reading C, but I like to use chat GPT to check to make sure Im on the right track.

Since I want this writeup to be beginner friendly, I am going to walk you through the main function.

The first thing that happes is that it checks argc (the number of arguments passed in the command line) is above 1, this basically tells the code that we didn't pass any arguments when we ran the binary from the terminal so it will tell you to pass the flag.
Next it creates a variable called void var_6d, and then copies [ UNATCO. Nano-augs key encryption v0.1 ] into var_6d, specifying a holding size of 42 (0x2a hex -> decimal). Next it creates a variable called var_43, and into it it copies 43 bytes of data stored at the address of data_20f0. What is this data? lets click it and see what it has stored:

![image](https://github.com/user-attachments/assets/758daaf8-0723-4d8a-a1be-9986de41bfb8)

It then creates a variable i which is set to the length of var_6d. Next an integer rdx is set to 0. (this is going to be a counter for the while loop) Now we have a while loop that runs while our counter is less than the length of var_6d, in this while loop, for each iteration it does an XOR operation on each of the characters in var_6d and var_43, and increments ourcounter, once our counter is the same length as the stored string length, the loop is finished and the two strings have been XORed together. There is a string str created that tells us we have successfully found the flag, however there is an if statement that says that if our input string (which should be the flag) is not equal to the XORed result, then change the string to say we are unauthorized. The code will then output one of those two strings and we are finished. 

Now that we know how the code works we know to get the flag we need to XOR those two stored strings, to do this I am going to use a python script:

xor_string1 = b"[ UNATCO. Nano-augs key encryption v0.1 ]"
#Convert this string to bytes for XOR 

xor_string2 = bytes.fromhex("19 75 19 02 3a 07 0d 7f 1e 70 11 35 26 5f 7e 52 2a 2b 42 62 39 51 2b 11 56 5b 3c 35 4a 24 2b 3d 27 5d 6d 29 7b 1d 68 15 20 51 00")
#convert to bytes for XOR operaton

xor_result = bytes([b1 ^ b2 for b1, b2 in zip(xor_string1, xor_string2)]) 
#XOR the bytes together to get the flag

print(xor_result.decode('ascii', errors='replace'))
#print the flag




Our result is: BULL{SN00P_TH0S3_L1BR4R135_G3T_TH3M_K3Y5}

If we were to submit this to the machine it would tell us we got flag 1/3. 

Thank you for reading my writeup I hope it was helpful. 



