Challenge: 

<img width="619" height="541" alt="image" src="https://github.com/user-attachments/assets/196999a9-85dd-4da9-8da4-b1d3db17cdc9" />

In this challenge we are going to hack a webstore that sells ZaZa.

Once we log in we see our options: 

<img width="1898" height="873" alt="image" src="https://github.com/user-attachments/assets/de14a2db-881d-4e3d-9e41-00e6ae22743d" />

We see that we only have 100$ and Real Za is 1000$ dollars, what can we do to get the real za? Lets look at the backend code.

When looking we see that it isn't too strict on what we send, so we can add any "product" to our cart:

<img width="689" height="624" alt="image" src="https://github.com/user-attachments/assets/1ebdb2c5-d922-4047-892f-2223dc9fb727" />

When checking out we see it uses the product to reference a price in an array: 

<img width="478" height="119" alt="image" src="https://github.com/user-attachments/assets/0bd7def8-beae-4c48-98a6-95569e34d520" />

The unique thing about javascript is that is is very dynamic, it isn't super strict, so instead of throwing an error, it will just keep running with a wrong value. 

<img width="661" height="58" alt="image" src="https://github.com/user-attachments/assets/a6d1cbd7-48f9-41f2-b62e-c96529243cc7" />

We see it checks to make sure we actually have enough money, but since this is javascript we are talking about, something unexpeected could happen.

We know that if we try to reference a price for a product that doesnt exist, we will get an undefined value or "NaN". What happens when NaN is compared with our balanec of 100$? 

<img width="1060" height="108" alt="image" src="https://github.com/user-attachments/assets/4b9119ea-4488-4c63-96cf-0accb7ceb6ca" />

So it returns false, which means it passes that if statement and gets processed! 

Lets try this out, I will use burpsuite to help me modify my request to send a fake product.

<img width="1912" height="950" alt="image" src="https://github.com/user-attachments/assets/8b9aa4cb-aa06-4896-b3f1-72cd36bac796" />

Here I modify the value, lets check out.

<img width="1892" height="729" alt="image" src="https://github.com/user-attachments/assets/69624d2f-75c9-4639-9019-c125d820f0ff" />

Looks like it fell for it

<img width="1305" height="328" alt="image" src="https://github.com/user-attachments/assets/d7992172-c724-49e0-b90f-e333cb794070" />

It worked, lets ckeck out our Real Za.

<img width="1822" height="806" alt="image" src="https://github.com/user-attachments/assets/58a51565-e93e-4b44-9b63-95dd5981e8e0" />

Here is our flag. 

Thank you for reading.
