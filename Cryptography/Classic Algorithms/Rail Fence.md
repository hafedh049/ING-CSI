# Rail Fence Cipher – Encryption and Decryption

Given a plain-text message and a numeric key, cipher/de-cipher the given text using Rail Fence algorithm.   
The rail fence cipher (also called a zigzag cipher) is a form of transposition cipher. It derives its name from the way in which it is encoded.   

****Examples:**** 

****Encryption****  
Input :  "GeeksforGeeks "  
Key = 3  
Output : GsGsekfrek eoe  
****Decryption****  
Input : GsGsekfrek eoe  
Key = 3  
Output :  "GeeksforGeeks "  
  
****Encryption****  
Input :  "defend the east wall"  
Key = 3  
Output : dnhaweedtees alf  tl  
****Decryption****  
Input : dnhaweedtees alf  tl  
Key = 3  
Output : defend the east wall  
  
****Encryption****  
Input : "attack at once"  
Key = 2   
Output : atc toctaka ne   
****Decryption****  
Input : "atc toctaka ne"  
Key = 2  
Output : attack at once  

****Encryption****

In a transposition cipher, the order of the alphabets is re-arranged to obtain the cipher-text. 

- In the rail fence cipher, the plain-text is written downwards and diagonally on successive rails of an imaginary fence.
- When we reach the bottom rail, we traverse upwards moving diagonally, after reaching the top rail, the direction is changed again. Thus the alphabets of the message are written in a zig-zag manner.
- After each alphabet has been written, the individual rows are combined to obtain the cipher-text.

For example, if the message is “GeeksforGeeks” and the number of rails = 3 then cipher is prepared as:   
 

![Rail Fence Algorithm](https://media.geeksforgeeks.org/wp-content/uploads/Untitled1.jpg)

.’.Its encryption will be done row wise i.e. GSGSEKFREKEOE

****Decryption****

As we’ve seen earlier, the number of columns in rail fence cipher remains equal to the length of plain-text message. And the key corresponds to the number of rails.  
 

- Hence, rail matrix can be constructed accordingly. Once we’ve got the matrix we can figure-out the spots where texts should be placed (using the same way of moving diagonally up and down alternatively ).
- Then, we fill the cipher-text row wise. After filling it, we traverse the matrix in zig-zag manner to obtain the original text.

Implementation:   
Let cipher-text = “GsGsekfrek eoe” , and Key = 3   
 

- Number of columns in matrix = len(cipher-text) = 13
- Number of rows = key = 3

Hence original matrix will be of 3*13 , now marking places with text as ‘*’ we get   
 

* _ _ _ * _ _ _ * _ _ _ *  
_ * _ * _ * _ * _ * _ *   
_ _ * _ _ _ *  _ _ _ * _   

```python
# Rail Fence Cipher Encryption and Decryption

# Function to encrypt a message using Rail Fence Cipher
def encryptRailFence(text, key):
    # Create the matrix for the cipher with '\n' to represent blank spaces
    rail = [['\n' for _ in range(len(text))] for _ in range(key)]

    # Direction flag to determine zigzag pattern
    dir_down = False
    row, col = 0, 0

    # Fill the matrix using the zigzag pattern
    for i in range(len(text)):
        # Change direction when hitting the top or bottom rail
        if row == 0 or row == key - 1:
            dir_down = not dir_down

        # Place the character in the matrix
        rail[row][col] = text[i]
        col += 1

        # Move to the next row according to direction
        if dir_down:
            row += 1
        else:
            row -= 1

    # Read the matrix row-by-row to get the encrypted text
    result = []
    for i in range(key):
        for j in range(len(text)):
            if rail[i][j] != '\n':
                result.append(rail[i][j])

    return "".join(result)

# Function to decrypt a message encrypted with Rail Fence Cipher
def decryptRailFence(cipher, key):
    # Create an empty matrix to mark positions of characters
    rail = [['\n' for _ in range(len(cipher))] for _ in range(key)]

    # Mark positions where characters will go using '*'
    dir_down = None
    row, col = 0, 0

    for i in range(len(cipher)):
        if row == 0:
            dir_down = True
        if row == key - 1:
            dir_down = False

        # Mark position with '*'
        rail[row][col] = '*'
        col += 1

        # Move to the next row according to direction
        if dir_down:
            row += 1
        else:
            row -= 1

    # Fill the matrix with actual characters from the cipher text
    index = 0
    for i in range(key):
        for j in range(len(cipher)):
            if rail[i][j] == '*' and index < len(cipher):
                rail[i][j] = cipher[index]
                index += 1

    # Read the matrix in a zigzag pattern to reconstruct the plaintext
    result = []
    row, col = 0, 0
    for i in range(len(cipher)):
        if row == 0:
            dir_down = True
        if row == key - 1:
            dir_down = False

        # Add character to the result
        if rail[row][col] != '\n':
            result.append(rail[row][col])
            col += 1

        # Move to the next row according to direction
        if dir_down:
            row += 1
        else:
            row -= 1

    return "".join(result)

# Driver code
if __name__ == "__main__":
    # Test encryption
    print(encryptRailFence("attack at once", 2))  # Output: "atc toctaka ne"
    print(encryptRailFence("GeeksforGeeks", 3))   # Output: "GsGsekfrek eoe"
    print(encryptRailFence("defend the east wall", 3))  # Output: "dnhaweedtees alf tl"

    # Test decryption
    print(decryptRailFence("GsGsekfrek eoe", 3))  # Output: "GeeksforGeeks"
    print(decryptRailFence("atc toctaka ne", 2))  # Output: "attack at once"
    print(decryptRailFence("dnhaweedtees alf tl", 3))  # Output: "defend the east wall"
```

**Time Complexity:*** O(row * col)  
**Auxiliary Space:** O(row * col)   
**References:**   
[https://en.wikipedia.org/wiki/Rail_fence_cipher](https://en.wikipedia.org/wiki/Rail_fence_cipher)  
This article is contributed by [****Ashutosh Kumar****](https://in.linkedin.com/in/ashutosh-kumar-9527a7105)
## **How It Works**

### **Encryption Process**

1. **Matrix Setup:**  
    The message is written diagonally across rows (rails), moving down and up in a zigzag pattern.
    
2. **Zigzag Placement:**  
    If the direction reaches the bottom rail, it switches to move upwards, and vice versa when it reaches the top rail.
    
3. **Read Row by Row:**  
    Once the entire message is placed in the zigzag pattern, we read the rows one-by-one to get the **encrypted text**.

### **Decryption Process**

1. **Mark the Matrix:**  
    During decryption, we first mark the zigzag positions with `*` to determine where each letter will go.
    
2. **Fill the Matrix:**  
    We place the characters from the cipher text into their corresponding positions.
    
3. **Read in Zigzag:**  
    Finally, we read the matrix in the same zigzag pattern as used during encryption to retrieve the **original message**.