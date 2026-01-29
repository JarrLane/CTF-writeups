This was a daily challenge from ICTF

Challenge: 

<img width="789" height="358" alt="image" src="https://github.com/user-attachments/assets/53fb6456-a958-4bc4-9ab5-44dc1d3a2752" />

When we go to the site, this is what we see: 

<img width="879" height="566" alt="image" src="https://github.com/user-attachments/assets/1e68b064-8a89-40b6-9fa0-e13ad9623526" />

Every time we click the button our count increases. Lets look at the code.

<img width="580" height="592" alt="image" src="https://github.com/user-attachments/assets/c5bde901-6994-4c0d-8ea5-fce17bf6a62e" />

What is important to us is the /click route. We see that when we click the button it checks our clicks with a cookie, and it sees if our clicks are above 10000000000, if so we get the flag.

We don't want to actually click 10000000000 times so lets break the website instead.

We know that the code checks a cookie that gets sent, lets look at a request through burpsuite: 

<img width="1573" height="884" alt="image" src="https://github.com/user-attachments/assets/319ac681-607a-499a-822e-0c337aeccda0" />

If you look closely, you will notice that our click cookie is sent, lets try editing it to a big number: 

<img width="1575" height="879" alt="image" src="https://github.com/user-attachments/assets/d745d0e9-17aa-4433-a887-8fb5d7afafbd" />

Lets see our result: 

<img width="847" height="263" alt="image" src="https://github.com/user-attachments/assets/5437d460-a0e1-4c4c-897e-a6b64f83758d" />

We get the flag!!! 

THank you for reading.
