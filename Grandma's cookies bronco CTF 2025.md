This is a web challenge that was a part of Bronco CTF 2025. I would say overall it was easy. 

This is what the challenge looks like: 

![image](https://github.com/user-attachments/assets/15cbde58-b227-4690-bddd-e732c5acc9c9)

We need to see the secret behind her recipie, so we begin by first looking at the website. 

![start screen](https://github.com/user-attachments/assets/ea0f9516-2d71-42b3-ad7f-9431c5b13f67)

We are interested in Grandma's pantry so we should look there.

![the problem](https://github.com/user-attachments/assets/a323ecea-b1e5-433e-8633-b14f80bac250)

It seems like we aren't allowed, since this challenge is about cookies, I think it may be a good idea to check the website cookies.

![KHcookies](https://github.com/user-attachments/assets/7a6437cc-defa-419f-8110-efda99983f7f)

It seems that we are labeled as the kitchen helper and we have a hash saved too, we want to be Grandma so the first thing to try is to see if changing the role to Grandma will work.

![just grandma doesnt work](https://github.com/user-attachments/assets/6719d2f4-249d-4d3d-ad45-6ecddf48e46c)

It does not work, looking into the cookies a bit more we can see why. Using a hash identifier, I found out that the hash in our cookies was an MD5 hash. I then thought that maybe the hash represented our intended role, so I went to an online website and found the hash of "kitchen helper". It turns out my theory was correct, so this means I need the hash to match Grandma, so I plugged in "Grandma" into MD5 and replaced the kitchen helpers hash with this hash.

Trying this again with the correct role and hash this is what we get. 

![winning screen](https://github.com/user-attachments/assets/20b27a96-735d-44f0-8237-ab11ece10b94)

Further notes: 

If Grandma wanted to have a reliable token that users couldn't easily change, she could've used JWT. JWT, or Json web token, works similarly to how this website tried to maintain access control, although it is much more secure. Using a secret key, A token containing a users information, token information, and a hash generated from the user and token information can be assigned to the user. If a user tries to modify the token without knowing the key, the system will be able to see that the information hashed with the intended key does not match the hash that was sent so it is invalid. While JWT is a great option for security, it has its own vulnerabilities that must be considered before implementing it, and also if a weak key is used, it can be brute forced and the information can be modified. 

Thank you for taking the time to read my writeup, I hope you were able to learn something.




