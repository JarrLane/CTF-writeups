# Noncesence

For this challenge we begin by connecting to a server via netcat, we are also given the python of the server.

```

class ChallengeHandler(socketserver.StreamRequestHandler):
    timeout = 30  # seconds
    def handle(self):
        try:
            enc_flag = encrypt(FLAG)
            self.wfile.write(b"Encrypted flag: " + enc_flag.hex().encode() + b"\n")
            self.wfile.write(b"Enter plaintext (hex): ")
            self.wfile.flush()
            raw = self.rfile.readline(513).strip().decode()
            data = bytes.fromhex(raw)[:256]
            enc_data = encrypt(data)
            self.wfile.write(b"Ciphertext: " + enc_data.hex().encode() + b"\n")
        except Exception:
            pass
```
Basically what it does is sends us the encrypted flag, then lets us send plaintext where it will send back the ciphertext

We also know it is using AES counter mode, which is a symmetric encryption operation. 

Lets look at the encrypt function: 

```

def encrypt(data: bytes) -> bytes:
    cipher = AES.new(KEY, AES.MODE_CTR, nonce=NONCE)
    return cipher.encrypt(data)
```

We see that each time we call the encryption function, it uses the same NONCE.

I could explain why using the same nonce is bad but this guy already did a good job of doing so: https://frereit.de/aes_gcm/

Since the server gives us 2 ciphertexts with the same nonce, we can extract what was xored with the plaintext then use that to see what the original flag is.

Here is what happens when we connect to the server:

<img width="873" height="116" alt="image" src="https://github.com/user-attachments/assets/588fbaf0-6847-4001-b794-8d1c66a91f02" />

For my plaintext I just sent the hex encoding of "pleasejustencryptthispleasejustencryptthis"

So now the server has given us 2 ciphertexts, and we know the plaintext of one. What we need to do first is find what actually gets XORed with the plaintext.

For this we need to XOR the plaintext we sent and ciphertext that the server returned from it. 

<img width="1529" height="711" alt="image" src="https://github.com/user-attachments/assets/8ad52ca0-e285-4fad-a273-af406c69fae5" />

This resulting value is our key from hex, now lets XOR it with the ciphertext of the flag:

<img width="1583" height="701" alt="image" src="https://github.com/user-attachments/assets/ebc7bac6-b348-4e6c-8444-f65260595d73" />

Here we get our flag. Thank you for reading.




