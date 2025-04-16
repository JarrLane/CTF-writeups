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
the attacker doesnt know the key, so ther server will know that sinec the modified message generated a different signature than what is expected when using the key, something is wrong
so the request is cancelled. In this challenge, we know the key, so now if we try modifying the JWT, we can generate a signature with the proper key, and when ther server checks the JWT 
with a hash algorithm, it will be valid which will continue the request.

Now with that explained its time to apply it to this website, we know the key already so now lets try changing the jwt. 


