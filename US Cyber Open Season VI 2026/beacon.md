# Beacon

For this challenge we are given an executable, lets check it out in binary ninja:

Here is our main function, we see there is a strange function that returns true or false, and this determines if our "override" is rejected

<img width="851" height="419" alt="image" src="https://github.com/user-attachments/assets/e438c452-ffb9-4f19-ab35-cd8450a48cf4" />

Checking out that function, we see some checks. This code might look weird at first but I will explain. The first check is making sure the length of what is input in the function is exactly 0x17 (or 23) characters.
We then see a while loop that iterates through the input, and for each character it checks if the result of a function being run on part of our input is equal to some data. It checks and compares each character of the input
and if they are all equal to the data then it returns true.

<img width="800" height="339" alt="image" src="https://github.com/user-attachments/assets/30acd1c1-35e8-4774-9015-091892ea7da6" />

Lets check out our new function that seems to be running an operation on each character of our input:

<img width="568" height="78" alt="image" src="https://github.com/user-attachments/assets/04b4921a-8d46-49d9-9238-ad2f3ffec57b" />

So this function takes in 2 inputs (we pass in the current character and the index of that character (also the counter of the while loop)). It adds the index number to our character and Xors it with 0x13. 

Supposedly, the result of this operation on the correct input data should be the data, lets look at the data:

<img width="1204" height="98" alt="image" src="https://github.com/user-attachments/assets/401aa336-5bbd-4281-aebd-7029c96ee90d" />

Now that we have all of this lets think in reverse. If the correct phrase is put through an operation where each letter gets XORed by 0x13 then has the index added, then using that resulting data, SUBTRACT 
the index value, XOR with 0x13, and get the original character.

This might sound confusing so let me walk you through two steps. The first character is @, since this is the 0th position (remember arrays start at 0) we dont subtract anything so now lets xor that with 0x13:

<img width="818" height="573" alt="image" src="https://github.com/user-attachments/assets/ac4e16f9-28fd-4b86-9aa4-20fd98d95a51" />

So we have S

Our next letter is F. F has a hex value of 46, lets subtract 1 since now the position is 1, and we get 45 or E, lets now XOR that:

<img width="1187" height="668" alt="image" src="https://github.com/user-attachments/assets/f23c2ac3-5a6d-4bb1-a142-2f578c9100b9" />

Now we have V, it looks like we are starting to get the flag.

For this challenge I used a script to make reversing much faster, I took the hex of the "Stored data" and ran it through a function that does what I just walked you through.

```
flag = "40465c5458466e78287b7a2e89593174307273358b3584"
flag_bytes = bytearray.fromhex(flag)

def getFlag():
    currentPos = 0
    decoded_chars = []

    while currentPos < len(flag_bytes):
        currentByte = flag_bytes[currentPos]
        minus = currentByte - currentPos
        nb = minus ^ 0x13
        
        decoded_chars.append(chr(nb))

        currentPos = currentPos + 1

    print("".join(decoded_chars))

getFlag()
```

The result is: SVIBGR{b3ac0n_0v3rr1d3}

Thank you for reading
