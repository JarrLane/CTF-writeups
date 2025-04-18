This challenge was from an in person event at USF (University of South Florida) called Hackabull, it was a hackathon and a CTF. I learned alot from this event and our
team won 3rd place. This was a great event and a good weekend overall.
![image](https://github.com/user-attachments/assets/7a7b00bf-18e7-4b18-ba9c-5ddd4df66a96)

This challenge tells us that this is definitely not SQL, lets check out the site.

If we try logging in with the given username and password, we see we are logged in and we are told we have a skill issue, now me personally I wouldn't take that disrespect.
![image](https://github.com/user-attachments/assets/0cfd2d36-0bc7-4c3f-b6e7-c39a1b98d7e8)
When looking around we find a json token, maybe this is a jwt challenge. 

Going back to the login page we find something very interesting: 

![image](https://github.com/user-attachments/assets/59138adc-680c-43a3-852e-608ef101d1a7)

They leaked the key to the jwt, that is so fascnating. For those who are unfamiliar with JWT (Json web token), here is a quick explanation: A JWT is a type of cookie that acts
like a personal token that your browser holds, and when you send a request this token is included. It is base 64 encoded and composed of a header, body, and hash. When decoding
a JWT token you can see what information the website holds about you stored in JSON format (a format that has a key & value object that is easy to read for both people and computers).
You might be wondering, whats stopping me from changing these values? This is where the hash/signature comes in. When a jwt cookie is sent it contains a hash that is generated from
the intended information that is supposed to be associated with a user. If an attacker tried to send a modified JWT, the cookie will change but the hash wont match the cookie because
the attacker doesnt know the key, so ther server will know that since the modified message generated a different signature than what is expected when using the key, something is wrong
so the request is cancelled. In this challenge, we know the key, so now if we try modifying the JWT, we can generate a signature with the proper key, and when ther server checks the JWT 
with a hash algorithm, it will be valid which will continue the request. It is worth noting that there are different algorithms that are used to sign JWT tokens, for our case it is HS256 which uses the same key to sign and verify.

Now with that explained its time to apply it to this website, we know the key already so now lets try changing the jwt.

It looks like the key is base 64 encoded, so lets decode it. 

![image](https://github.com/user-attachments/assets/4ef7b6db-5473-469a-add9-cc6e5530dd68)

We see the key is NightCity. Originally I was confused because I only decoded it once, but when i used the magic wand it automatically decoded it 3 times, use the magic wand tool if in doubt because there is a chance it can find the proper way to decode. The magic wand tool in cyber chef is when cyberchef tries to automatically determine which encoding is used, sometimes it helps sometimes it is wrong. In this case it definitely helped.

![image](https://github.com/user-attachments/assets/50662577-cd9f-4aa6-b06c-9ae7d7ecf151)

Here we can see the decoded JWT, we can see the admin is set to false, we want that to be true. (Note the 3rd part is the signature thats why it looks so weird.)

By using the JWT sign tool, we input the header, key, and body, and it will give us a new token
![image](https://github.com/user-attachments/assets/bbca09c9-bd7d-444e-baf6-c0ca17f2726d)

Now in the cookie section, I will replace with the new signed token and try to access an admin area.
![image](https://github.com/user-attachments/assets/c61a8c54-b4ff-4c30-9d49-5820e2c8c1b1)

There is the flag.

Thank you for reading my writeup I hope it was helpful. 










