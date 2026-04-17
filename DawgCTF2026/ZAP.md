This writeup covers both ZAP parts one and two.

# Zap 1

Here is part 1: 
<img width="1085" height="283" alt="image" src="https://github.com/user-attachments/assets/294eb750-a008-4ce9-a852-2d736b68fba6" />

So theres this weird electrical thing, lets take a look:
<img width="2621" height="3938" alt="zap1" src="https://github.com/user-attachments/assets/0592a4b3-decb-4237-8877-9292d5d63f06" />
<img width="4032" height="3024" alt="zap3" src="https://github.com/user-attachments/assets/221375f6-d029-4dd3-aed1-eb26fae8b9d3" />
<img width="4284" height="5712" alt="zap2" src="https://github.com/user-attachments/assets/47866dc0-acce-488b-9182-6d9aefc5ff97" />


What even is this? Well lets look at what the NIA is and what an ST number is. Googling tells me there is group caled the National Insulator Association. 

They have a website too: https://www.nia.org/

This is a group focused on studying, collecting, and classifying electrical insulators, thats pretty cool, but what is an ST number?

Googling more tells me ST means suspention type, which is a way of classifying suspension insulators by certain characteristics.

https://www.nia.org/general/suspensions/#:~:text=Suspension%20Insulators,the%20following%20ranges%20by%20type:

Knowing this lets take a closer look at the given pictures. If we look closely we see engravings that say "1800","20000","Locke",and "GE USA". Lets look into these more. Researching we find that 20000 is its mechanical tension and that Locke is a maufacturer of electric insulator. From here I did more research and saw a listing of insulators in the ST range of ST-4500-4749:

https://allinsulators.com/photos/ST/4500-4749.php

I decided to look further and see if I could find the same insulator. At first I thought it was ST-4626A because they looked very similar so I tried that but it wasn't it. My next best guess was ST-4626F because despite the different color it looked the same and that was the correct one. 

Flag: DawgCTF{ST-4626F}

# Zap 2

Here is zap 2:
<img width="1085" height="268" alt="image" src="https://github.com/user-attachments/assets/81bb685b-1e01-47d1-aeaa-2bce26f92e33" />

So now we need to find the model number, with all of the details from earlier I started searching all over.

Eventually I randomly stumbled across this page: 
https://www.scribd.com/document/739618993/Locke-Cap-Pin-Insulators

According to the description:

"This document provides specifications and characteristics for various types of Locke suspension insulators for different voltage ratings. It includes details on insulator models, ANSI ratings, mechanical strength, leakage distances, flashover voltages, and more technical specifications."

That is exactly what we want, lets look at this document more. 

<img width="802" height="474" alt="image" src="https://github.com/user-attachments/assets/62f10e1c-595b-4c20-8c9c-630774f1457e" />

Here on this page we see a table and it has 20k, thats what our model is, I decided to try 20S840 because it had the same structure as the example flag so it must be a model number. Trying that I got the flag.

Flag: DawgCTF{20S840}

Overall these were fun challenges and it was an interesting rabit hole to go down, thank you for reading my writeup.

