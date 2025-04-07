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

This website uses Mongo DB, a NoSQL data base that uses BSON to store and transmit data. BSON is a binary encoded serialization of JSON. (For those who dont know, JSON is basically a format for storing/transmitting data that is easy to read, exchange, and parse.) When we say serialize we mean to take an object and format it so that it can be easily transmitted. When we say deserialize we mean reformatting the transmitted data back into its original object. 

Sometimes there can be inconsistencies between serialized and unserialized data, and this can cause vulnerabilities. More specifically, 

