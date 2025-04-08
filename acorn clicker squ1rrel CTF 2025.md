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

This website uses Mongo DB, a NoSQL data base that uses BSON to store and transmit data. BSON is a binary encoded serialization of JSON. (For those who dont know, JSON is basically a format for storing/transmitting data that is easy to read, exchange, and parse.) When we say serialize we mean to take an object and format it so that it can be easily transmitted/stored. When we say deserialize we mean reformatting the transmitted/stored data back into its original object. 

Sometimes there can be inconsistencies between serialized and unserialized data, and this can cause vulnerabilities. More specifically, in how BSON handles types with specific settings. In our case, BSON was being run with
an option called useBigInt64. In our code, when a number is sent, it is converted into a bigInt value, then incremented with the amount and stored in a serialized BSON form for Mongo DB. When it is then deserialized from BSON in order to send back the amount, the number will be sent back as a BigInt. This is a problem because the useBigInt setting might treat the stored negative amount differently.

What happens if I send -1 to the server? 
![sc1](https://github.com/user-attachments/assets/57f623d7-fe49-4d42-abdc-de7f52272b19)

This is the result: 
![image](https://github.com/user-attachments/assets/a6701821-6501-42dd-a6b6-3d8a3bcda016)

Now I can buy the flag: 

![image](https://github.com/user-attachments/assets/43a58c60-7bfe-42b5-bf46-6e04da72c3ff)

For security, whenever using applications that rely on different formatting, make sure values are interpreted consistantly to avoid bugs with how data is processed and stored. Avoid using the BSON library versions 6.4-6.10.2 with useBigInt64 enabled.

Originally I solved this the unintended way by thinking if I just send a ton of big negative values it would eventually loop around and go to a positive value and I somehow got that to work, but then I asked the organizers what the intended vulnerability was and found out it was in the way BSON reads negative numbers as big positive ones. 

This is the vulnerability: 

https://jira.mongodb.org/browse/NODE-6764

This was a great event and I learned alot from this challenge, thank you for reading my writeup.






