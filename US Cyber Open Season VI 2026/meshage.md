So for this challenge we have a website that processes 3d mesh.

<img width="1920" height="928" alt="image" src="https://github.com/user-attachments/assets/127f8f4e-4614-4858-88e4-b1b3a09f7db0" />

Before checking out the For Devs section, lets look at the security policies. We see directory indexing is disabled, which means the web server wont just display the filesystem if there is no index. We also see that models
are exported into backup blockes that end in .sys

Now going to the dev button we see:

<img width="338" height="169" alt="image" src="https://github.com/user-attachments/assets/d998d037-77dc-4cba-a9bd-8d04aebc8220" />

This just revealed another part of the website to us, now we can visit that subdirectory. We want to visit /assets_production_system_v3/

When we visit this subdirectory we find a folder called bak and a sys file, this means we found where the 3d models are backed up to

Lets check out this sys file in cyberchef

<img width="782" height="453" alt="image" src="https://github.com/user-attachments/assets/a7e96d82-6ddc-4750-a1eb-f2faa242751e" />

It seems we have what looks like a certificate, it looks like the data in the certificate is encoded in some way, lets try decoding with base 64:

<img width="1533" height="885" alt="image" src="https://github.com/user-attachments/assets/e9be9483-b273-4e8e-8f0e-e3b8f06c3560" />

We find what seems to be the header of an openscad file, lets try getting rid of those certificate headers and saving the file as openscad.

Once we do this lets try opening it up in a 3d software:

Using a free online viewer: 

<img width="1223" height="399" alt="image" src="https://github.com/user-attachments/assets/453102dc-fe40-4df6-a9ad-3034d7c0dfc0" />

We get the flag, thank you for reading.
