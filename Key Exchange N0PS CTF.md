This  a cryptography challenge that was part of N0PS ctf 2025. Usually I don't do many crypto challenges, although since I was already familiar with Diffie Hellman and AES, I decided to try it out and I am glad
that I did, this was a good experience for leveling up my skills in other challenge categories.

![image](https://github.com/user-attachments/assets/4368f70f-91c8-4f06-b9d0-19d0e6662ea2)

To begin lets look at the given python code, and I will explain what it does.
```
from Crypto.Util import number
from random import randint
from Crypto.Random import get_random_bytes
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
from hashlib import sha256
import sys

N = 1024

def gen_pub_key(size):
    q = number.getPrime(size)
    k = 1
    p = k*q + 1
    while not number.isPrime(p):
        k += 1
        p = k*q + 1
    h = randint(2, p-1)
    g = pow(h, (p-1)//q, p)
    while g == 1:
        h = randint(2, p-1)
        g = pow(h, (p-1)//q, p)
    return p, g

def get_encrypted_flag(k):
    k = sha256(k).digest()
    iv = get_random_bytes(AES.block_size)
    data = open("flag", "rb").read()
    cipher = AES.new(k, AES.MODE_CBC, iv)
    padded_data = pad(data, AES.block_size)
    encrypted_data = iv + cipher.encrypt(padded_data)
    return encrypted_data

if __name__ == '__main__':
    p, g = gen_pub_key(N)
    a = randint(2, p-1)
    k_a = pow(g, a, p)
    sys.stdout.buffer.write(p.to_bytes(N))
    sys.stdout.buffer.write(g.to_bytes(N))
    sys.stdout.buffer.write(k_a.to_bytes(N))
    sys.stdout.flush()
    k_b = int.from_bytes(sys.stdin.buffer.read(N))
    k = pow(k_b, a, p)
    sys.stdout.buffer.write(get_encrypted_flag(k.to_bytes((k.bit_length() + 7) // 8)))
```

This is what the code is doing: 

-Imports libraries that allows for cryptographic operations, random numbers, and interactiong with the system
*Defines a size
+Generates the public numbers p and g that will be used in further calculations with gen_pub_key(size)
-Generates a random private key between 2 and p-1
-Calculates a public key
-Write the p, g, and public key values to a buffer
-Flushes the buffer, sending out the p, g, and public key value to us
-Receives our public key
-Calculates the shared key with its private key and our public key
-Runs a function called get_encrypted_flag() that takes in the shared key, takes a hash of it, uses that hash to set up an AES CBC encryption operation, pads the data, uses AES CBC on the flag file 
and appends the IV used in front, and returns that value, that value is then sent to us, so we receive the IV and the encrypted flag

For those who are not aware of Diffie Hellman or AES CBC, 

## Diffie Hellman

I reccomend this video, this guy explains it very well, he's the one that helped me understand it, it is very worth the watch: https://www.youtube.com/watch?v=85oMrKd8afY

## AES CBC

AES (Advanced Encryption Standard) is a block cipher that takes in blocks of plaintext and encrypts them. CBC (Cipher Block Chaining) is a mode of operation used with block ciphers like AES.
You might be wondering, what is a mode of operation? Think about this, if we have a lot of plain text, we have a lot of blocks to encrypt, so how would we go about encrypting multiple blocks?
Well, one way you may be thinking is "lets just use the AES algorithm and key we generated on each blocks". This is a simple way to do it, although it is not the most secure because if the secret
key is found, all of the ciphertext is decrypted. This is where modes of operation come in, its a way of encrypting multiple blocks in a way that doesn't just rely on the key. CBC is a way to do this, How
does this work? Here is a quick visual I made in  blender: 

![image](https://github.com/user-attachments/assets/aabeeb7f-d5f9-4993-8b5d-bc09ac8bd8b0)

Notice how instead of directly passing the plaintext into AES, we XOR it with something called an IV, and then on the next block we XOR the plaintext with the previous ciphertext result. The IV 
(Initialization Vector) is basically a random number of a fixed size that we use to XOR our first plaintext block. Why use an IV? If we want to begin CBC, how can we start it if we don't have
a previous cipher block? This is why we use the IV, then for subsequent encryption the previous ciphertext get used. Notice how now, you cant't decrypt the ciphertext without knowing the IV and
the key. Also since the previous cipher text is used with the next block, all of the blocks are dependent on each other.

I hope I was able to summarize this well, now back to the ctf.

So now we need to communicate with the server to get the flag, a good way to do this is python, here is the script I used:

```
from pwn import remote
from Crypto.Util import number
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
from hashlib import sha256

N = 1024

conn = remote("0.cloud.chals.io", 26625)

data = b""
while len(data) < 3 * N:
    data += conn.recv(3 * N - len(data))

print(f"Received (length={len(data)}): {data.hex()}")

p_bytes = data[:N]
g_bytes = data[N:2*N]
k_a_bytes = data[2*N:3*N]

p = int.from_bytes(p_bytes, 'big')
g = int.from_bytes(g_bytes, 'big')
k_a = int.from_bytes(k_a_bytes, 'big')

print(f"p: {p}")
print(f"g: {g}")
print(f"k_a: {k_a}")

b = number.getRandomRange(2, p - 1)
k_b = pow(g, b, p)
k_b_bytes = k_b.to_bytes(N, 'big')

conn.send(k_b_bytes)

shared_key = pow(k_a, b, p)
aes_key = sha256(shared_key.to_bytes((shared_key.bit_length() + 7) // 8, 'big')).digest()
print(f"Shared key: {shared_key}")
print(f"AES key: {aes_key.hex()}")

enc = conn.recvall()

print(f"Response lentgth: {len(enc)}")

iv = enc[:16]
ciphertext = enc[16:]
print(f"IV length: {len(iv)}")
print(f"Ciphertext length: {len(ciphertext)}")


try:
    cipher = AES.new(aes_key, AES.MODE_CBC, iv)
    decrypted = cipher.decrypt(ciphertext)
    flag = pad(decrypted, AES.block_size)
    print(f"Flag: {flag}")
    with open("flag.png", "wb") as f:
        f.write(flag)
except ValueError as e:
    print(f"Decryption failed: {e}")
```
Here is what my code is doing: 

-Importing libraries that allow me to connect to the server and perform cryptographic operations (Also some advice, use the pycryptodome library not the crypto one, you will thank me later)
-Defining a size
-Connecting to the challenge server 
-Receive the data from the server (the shared values p g and public key)
-Calculate my own private key
-Use the public values and my private key to generate my own public key to send back to the server
-Send my public key to the server so the server can calculate our shared key
-Use the values I have to calculate the shared key for myself (At this point both me and the server will be using the same key)
-Do a SHA 256 hash on the shared key (Since that is what the server uses as the AES key to encrypt the file)
-Receive the response from the server which gives us the IV it used and the ciphertext that was generated
-Split the response into the IV and ciphertext so we can use it for decryption
-Set up the decryption with the key and IV that we now have
-Decrypt the ciphertext and pad it 
-Create a png file and put in the decrypted data (Why a png file? I will explain)
-(If there was an error with the decryption then the except statement gracefully tellsme)

So this script decrypts an image the server sent me, but how do I know that this is an image? A few ways.

![image](https://github.com/user-attachments/assets/1b2c9315-8bd7-4e90-8052-203cd9300b3b)

If we look at the raw bytes sent, we see PNG at the beginning

Here is another good way with cyberchef: 

If we take the first few hex values of the flag and put it in cyberchef, we can do this:

![image](https://github.com/user-attachments/assets/34ac378c-3849-4d1d-832d-995d0263e40e)

We can also use something called magic numbers, these are hex values that tell a computer which type of data is in the file, or how the file is meant to be formatted, the magic numbers for PNG are 89 50 4E 47 which is what 
we see in the hex.

When you open up the flag you get this:

I will wait until after the CTF ends to post the image



This was a very fun ctf and I am glad I was able to complete a category that I am not as familiar with. Thank you for reading my writeup I hope it helped.

