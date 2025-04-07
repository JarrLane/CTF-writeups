Challenge: 
![image](https://github.com/user-attachments/assets/ab7174b5-90f9-446a-ae2b-8be9d1eb84b6)

In this challenge, we are visiting a website where we click an acorn, and receive 1-10 acorns, and we can buy 3 squirrels with them, 
once we register an account we arrive at this page:

![image](https://github.com/user-attachments/assets/d1c6bb86-d9c5-4a85-aa61-133a95347eb9)

We want the flag, but it costs a lot and it would take too long to keep clicking the acorn

Looking at the provided code we see how clicking the acorn is handled: 

![image](https://github.com/user-attachments/assets/079e4b87-896a-44f3-8199-d97c131d74aa)

The server checks if the amount sent to the server is a number that is below 10, and then it will add the amount to our balance stored in mongo DB. This may seem like normal code 
but there is a vulnerability present.

Signed integer underflow is when a binary number can only represent the numbers in the range -(2^(n-1)-1) to (2^(n-1)-1), and when a program encounters a number beyond this range,
it will wrap around.

Here is an an example:

Lets say we have a limit of 4 binary digits, the range of numbers that can be covered are 2^(4-1)-1, so from -7 to 7. What happens if we take -6 (1010) and add -3 (1101), 
we get -9 (10111) since we can only store 4 digits the 1 is ignored and we get 0111, which evaluates to 7. 

The website only checks that we dont go bove 10 acorns, but it doesnt stop us from sending very large negative numbers, so if I keep sending large negative numbers
I will eventually loop back to a very big number and be able to buy the flag squirrel.

Will finish writeup in a bit




