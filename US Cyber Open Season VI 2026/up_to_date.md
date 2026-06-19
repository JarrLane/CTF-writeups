# Up To Date

Im going to be honest this was probably my favorite challenge

You basically reverse engineer this protocol to get the original message, JUST to reverse engineer that message.

So we are given python code and an output, first lets run through the code

We see a big function called chall, first we see:

```
random.seed(42)
zenc_code = """REDACTED"""
```

This is the random generator with seed 42 and the actual message that will be sent using this protocol

Next we see how the message is split up:

```
# Split the Zen-C code into five chunks
num_strides = 5
chunks = [zenc_code[i::num_strides] for i in range(num_strides)]
```

This is basically splitting the message into 5 chunks where each chunk gets a beginning character index + every 5 characters after that, youll see what this looks like soon

Now lets look at how this protocol is constructed:

```
packets = []
    # Valid packets: [Magic: ZN (2 bytes)][Seq (1 byte)][Len (1 byte)][Payload (Len bytes)]
    for seq_id, text in enumerate(chunks):
        # Secure the payload by starting it with its sequence ID
        payload_str = str(seq_id) + text
        payload = payload_str.encode('utf-8')
        header = b'ZN' + bytes([seq_id, len(payload)])
        packets.append(header + payload)
```
We see that a packet of this protocol has a magic number (basically bytes that identify what type of file, protocol, etc some data is), sequence number, length, and actual payload (the data we are trying to send) with 
the sequence number prepended

Seems simple but there are two functions that make reversing the transmission difficult.

Here is our first problem:

```
 # Noisy packet 1: Wrong magic bytes
    for _ in range(10):
        bad_magic = bytes(random.choices(range(256), k=2))
        if bad_magic == b'ZN':
            bad_magic = b'XX'
        rand_len = random.randint(5, 25)
        packets.append(bad_magic + bytes([random.randint(0, 255), rand_len]) + random_payload(rand_len))
```
It will send packets with incorrect magic bytes that dont actually mean anything and are just added to the transmission to be noisy

Here is our second problem:

```
# Noisy packet 2: Wrong payload start (not matching with sequence ID)
    for _ in range(10):
        dup_seq = random.randint(0, num_strides - 1)
        rand_len = random.randint(5, 25)
        
        bad_payload = random_payload(rand_len)
        # Make sure the bad payload doesn't start with correct sequence ID
        if bad_payload.startswith(str(dup_seq).encode('utf-8')):
            bad_payload = b'X' + bad_payload[1:]
            
        packets.append(b'ZN' + bytes([dup_seq, len(bad_payload)]) + bad_payload)
```
Here it will make bad packets that have duplicate sequence numbers that will confuse us into not knowing which of the packets with the same sequence number are actually relavent or just garbage data.


From there it shuffles the packets and combines them to a big hex output then sends it all
```
random.shuffle(packets)
full_stream = b"".join(packets)
hex_output = full_stream.hex()
```


So knowing all of this, we know we are going to read the output of the transmission and try to reconstruct the original message while getting through the noisy packets.

I dont want to go through the entire process of how I filtered out the packets so I will give you an example of a good packet, noisy packet 1, and noisy packet 2

Good packet: 

<img width="1208" height="654" alt="image" src="https://github.com/user-attachments/assets/553774f4-0df7-4f54-9274-84db6fa470b4" />

Here we see the right magic bytes, as well as consistent sequence numbers (31 is 1 in hex)

Noisy packet 1:

So for this one I used a little trick. Since they give us the seed (42), we can find and eliminate all of the noisy packets type 1:

```
import random
import string

random.seed(42)
for i in range(10):
    bad_magic = bytes(random.choices(range(256), k=2))
    
    APPENDED = random.randint(5, 25)   
    
    rByte = random.randint(0, 255)
    
    rPayload = random.choices(string.ascii_letters + string.digits, k=APPENDED)
    
    
    print(bad_magic)
    
    print(APPENDED)
    
    print(rByte)
    print(rPayload)
```
Also side note keep in mind with random, think of it like a generator, the seed generates a ton of values that are used one by one to randomize something each time random is used one way or another

Anyways the first output of this revealed this here: 

<img width="1560" height="869" alt="image" src="https://github.com/user-attachments/assets/1a15be90-5b0b-4e71-ab3c-10999da9e32f" />

So thats an exxample of noisy packet 1

Noisy packet 2: 

This one is pretty easy to notice, look at this packet:

<img width="856" height="606" alt="image" src="https://github.com/user-attachments/assets/9e00a77b-727b-4a1d-9bed-484c518abd63" />

You see the the duplicated sequence number 02 but the prepended value is t (0x74), so I know its fake

Anyways now here are the packets that I filtered out:

<img width="600" height="295" alt="image" src="https://github.com/user-attachments/assets/84b59a37-91d1-416d-a0f8-b07c155a331b" />

So with these valid packets we need to reconstruct the message, since from earlier we know how it was split into 5 arrays, I made a python script to reconstruct the message

```
chunk1 ="3066692d692065645b2c34307830202c39337830202c31317830202c31317830202c33317830202c663320657978206f69647028286b3b0a"
chunk2 ="316e6e3e6420746530202c62307830202c38377830202c64337830202c64327830202c64327830202c662074203420726e657263626520"
chunk3 ="32202820202020207830202c30307830202c31327830202c34377830202c36327830202c35337830205d20203d3220202020696820797d"
chunk4 ="336d29767b20633d317830202c35317830202c63317830202c33327830202c33377830202c303778303b206b203b2062637b6e615e290a"
chunk5 ="3461206f0a6c6f2031317830202c30337830202c64327830202c30377830202c35327830202c3632780a6c65300a66206f20747220297d"

chunk1Bytes = bytearray.fromhex(chunk1)
chunk2Bytes = bytearray.fromhex(chunk2)
chunk3Bytes = bytearray.fromhex(chunk3)
chunk4Bytes = bytearray.fromhex(chunk4)
chunk5Bytes = bytearray.fromhex(chunk5)
chunks = [chunk1Bytes, chunk2Bytes, chunk3Bytes, chunk4Bytes, chunk5Bytes]
reconstructed = []

min_length = min(len(c) for c in chunks)

for i in range(min_length):
    for chunk in chunks:
        reconstructed.append(chunk[i])

decoded_string = bytes(reconstructed).decode('utf-8', errors='ignore')
print(decoded_string)
```

Here is our decoded message:

```
01234fn main() -> void {
    let code = [0x11, 0x14, 0x0b, 0x00, 0x05, 0x10, 0x39, 0x38, 0x71, 0x2c, 0x1d, 0x21, 0x1d, 0x34, 0x73, 0x20, 0x71, 0x1d, 0x26, 0x23, 0x75, 0x23, 0x1d, 0x25, 0x30, 0x76, 0x2f, 0x3f];
    let key = 0x42;
    for b in code { print(char(b ^ key)); }
}
```
Looks like we have to reverse even more code, but this time it looks very simple, just a string that needs to be XORed, lets go ahead and do that.

<img width="1339" height="569" alt="image" src="https://github.com/user-attachments/assets/838ffada-354f-40c9-851d-157e3a17af7b" />

We got the flag. Thank you for reading



