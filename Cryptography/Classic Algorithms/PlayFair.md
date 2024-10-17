# Playfair Cipher with Examples

Last Updated : 30 Jul, 2024

The ****Playfair cipher**** was the first practical digraph substitution cipher. The scheme was invented in ****1854**** by ****Charles Wheatstone**** but was named after Lord Playfair who promoted the use of the cipher. In playfair cipher unlike [traditional cipher](https://www.geeksforgeeks.org/caesar-cipher/) we encrypt a pair of alphabets(digraphs) instead of a single alphabet.  
It was used for tactical purposes by British forces in the Second Boer War and in World War I and for the same purpose by the Australians during World War II. This was because Playfair is reasonably fast to use and requires no special equipment.  
 

Encryption Technique

  
For the encryption process let us consider the following example:  
 

![Encryption Technique](https://media.geeksforgeeks.org/wp-content/uploads/20190818180100/example-playfair.png)

  
****The Playfair Cipher Encryption Algorithm:****   
The Algorithm consists of 2 steps:   
 

1. ****Generate the key Square(5×5):****   
    - The key square is a 5×5 grid of alphabets that acts as the key for encrypting the plaintext. Each of the 25 alphabets must be unique and one letter of the alphabet (usually J) is omitted from the table (as the table can hold only 25 alphabets). If the plaintext contains J, then it is replaced by I.   
         
    - The initial alphabets in the key square are the unique alphabets of the key in the order in which they appear followed by the remaining letters of the alphabet in order.   
         
2. ****Algorithm to encrypt the plain text:**** The plaintext is split into pairs of two letters (digraphs). If there is an odd number of letters, a Z is added to the last letter.   
    ****For example:****   
     

****PlainText****: "instruments"   
****After Split:**** 'in' 'st' 'ru' 'me' 'nt' 'sz'  

****1.**** Pair cannot be made with same letter. Break the letter in single and add a bogus letter to the previous letter.

****Plain Text:**** “hello”

****After Split:**** ‘he’ ‘lx’ ‘lo’

Here ****‘x’**** is the bogus letter.

****2.**** If the letter is standing alone in the process of pairing, then add an extra bogus letter with the alone letter

****Plain Text:**** “helloe”

****AfterSplit:**** ‘he’ ‘lx’ ‘lo’ ‘ez’

Here ****‘z’****  is the bogus letter.

  
****Rules for Encryption:**** 

- ****If both the letters are in the same column****: Take the letter below each one (going back to the top if at the bottom).  
    ****For example:****   
     

****Diagraph:**** "me"  
****Encrypted Text:**** cl  
****Encryption:****   
  m -> c  
  e -> l

![Rules for Encryption 1](https://media.geeksforgeeks.org/wp-content/uploads/20190818175431/encryption-of-me.png)

- ****If both the letters are in the same row****: Take the letter to the right of each one (going back to the leftmost if at the rightmost position).  
    ****For example:****   
     

****Diagraph:**** "st"  
****Encrypted Text:**** tl  
****Encryption:****   
  s -> t  
  t -> l

![Rules for Encryption 2](https://media.geeksforgeeks.org/wp-content/uploads/20190818175435/encryption-of-st.png)

- ****If neither of the above rules is true****: Form a rectangle with the two letters and take the letters on the horizontal opposite corner of the rectangle.

****For example:****  

****Diagraph:**** "nt"  
****Encrypted Text:**** rq  
****Encryption:****   
  n -> r  
  t -> q

![Rules for Encryption 3](https://media.geeksforgeeks.org/wp-content/uploads/20190818175433/encryption-of-nt.png)

****For example:**** 

****Plain Text:**** "instrumentsz"  
****Encrypted Text:**** gatlmzclrqtx  
****Encryption:****   
  i -> g  
  n -> a  
  s -> t  
  t -> l  
  r -> m  
  u -> z  
  m -> c  
  e -> l  
  n -> r  
  t -> q  
  s -> t  
  z -> x

![Example of encryption](https://media.geeksforgeeks.org/wp-content/uploads/20190818175428/encryption-of-instruments.png)


```python
# Function to convert the string to lowercase
def toLowerCase(text):
    return text.lower()

# Function to remove all spaces in a string
def removeSpaces(text):
    return "".join(text.split())

# Function to group the string into pairs of two elements
def Diagraph(text):
    diagraph = []
    group = 0
    for i in range(2, len(text), 2):
        diagraph.append(text[group:i])
        group = i
    diagraph.append(text[group:])
    return diagraph

# Function to add filler letters if a pair contains the same character
def FillerLetter(text):
    k = len(text)
    new_word = text
    for i in range(0, k - 1, 2):
        if text[i] == text[i + 1]:
            new_word = text[:i + 1] + 'x' + text[i + 1:]
            return FillerLetter(new_word)
    return new_word

# List of characters used in the 5x5 matrix (excluding 'j')
list1 = [
    'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'k', 
    'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 
    'v', 'w', 'x', 'y', 'z'
]

# Function to generate the 5x5 key square matrix
def generateKeyTable(word, list1):
    key_letters = []
    for char in word:
        if char not in key_letters:
            key_letters.append(char)

    compElements = key_letters + [ch for ch in list1 if ch not in key_letters]

    matrix = [compElements[i:i + 5] for i in range(0, len(compElements), 5)]
    return matrix

# Function to search for the position of a character in the matrix
def search(mat, element):
    for i in range(5):
        for j in range(5):
            if mat[i][j] == element:
                return i, j

# Encryption rules based on the row
def encrypt_RowRule(matr, e1r, e1c, e2r, e2c):
    char1 = matr[e1r][(e1c + 1) % 5]
    char2 = matr[e2r][(e2c + 1) % 5]
    return char1, char2

# Encryption rules based on the column
def encrypt_ColumnRule(matr, e1r, e1c, e2r, e2c):
    char1 = matr[(e1r + 1) % 5][e1c]
    char2 = matr[(e2r + 1) % 5][e2c]
    return char1, char2

# Encryption rules when characters form a rectangle
def encrypt_RectangleRule(matr, e1r, e1c, e2r, e2c):
    char1 = matr[e1r][e2c]
    char2 = matr[e2r][e1c]
    return char1, char2

# Function to encrypt text using Playfair Cipher
def encryptByPlayfairCipher(matrix, plainList):
    cipherText = []
    for pair in plainList:
        ele1_x, ele1_y = search(matrix, pair[0])
        ele2_x, ele2_y = search(matrix, pair[1])

        if ele1_x == ele2_x:
            c1, c2 = encrypt_RowRule(matrix, ele1_x, ele1_y, ele2_x, ele2_y)
        elif ele1_y == ele2_y:
            c1, c2 = encrypt_ColumnRule(matrix, ele1_x, ele1_y, ele2_x, ele2_y)
        else:
            c1, c2 = encrypt_RectangleRule(matrix, ele1_x, ele1_y, ele2_x, ele2_y)

        cipherText.append(c1 + c2)
    return cipherText

# Main Program
text_Plain = 'instruments'
text_Plain = removeSpaces(toLowerCase(text_Plain))
plainTextList = Diagraph(FillerLetter(text_Plain))

# Add 'z' if the last group has only one character
if len(plainTextList[-1]) != 2:
    plainTextList[-1] += 'z'

key = "Monarchy"
print("Key text:", key)

key = toLowerCase(key)
matrix = generateKeyTable(key, list1)

print("Plain Text:", text_Plain)
cipherList = encryptByPlayfairCipher(matrix, plainTextList)

cipherText = "".join(cipherList)
print("CipherText:", cipherText)

```

  
**Output**

Key text: Monarchy
Plain text: instruments
Cipher text: gatlmzclrqtx

Decryption Technique

Decrypting the Playfair cipher is as simple as doing the same process in reverse. The receiver has the same key and can create the same key table, and then decrypt any messages made using that key.  
 

![Decryption Technique](https://media.geeksforgeeks.org/wp-content/uploads/20190821185125/example-playfair-decryption.png)

  
****The Playfair Cipher Decryption Algorithm:****   
The Algorithm consists of 2 steps: 

1. ****Generate the key Square(5×5) at the receiver’s end:****   
    - The key square is a 5×5 grid of alphabets that acts as the key for encrypting the plaintext. Each of the 25 alphabets must be unique and one letter of the alphabet (usually J) is omitted from the table (as the table can hold only 25 alphabets). If the plaintext contains J, then it is replaced by I.   
         
    - The initial alphabets in the key square are the unique alphabets of the key in the order in which they appear followed by the remaining letters of the alphabet in order.   
         
2. ****Algorithm to decrypt the ciphertext:**** The ciphertext is split into pairs of two letters (digraphs).   
     

> ****Note****: The ****ciphertext**** always have ****even**** number of characters.

- ****For example:****   
     

****CipherText****: "gatlmzclrqtx"   
****After Split:**** 'ga' 'tl' 'mz' 'cl' 'rq' 'tx'  

- ****Rules for Decryption:**** 
    - ****If both the letters are in the same column****: Take the letter above each one (going back to the bottom if at the top).  
        ****For example:**** 

****Diagraph:**** "cl"   
****Decrypted Text:**** me  
****Decryption:****   
  c -> m  
  l -> e

- ![algorithm to decrypt the ciphertext](https://media.geeksforgeeks.org/wp-content/uploads/20190818175431/encryption-of-me.png)****If both the letters are in the same row****: Take the letter to the left of each one (going back to the rightmost if at the leftmost position).  
    ****For example:****   
     

****Diagraph:**** "tl"   
****Decrypted Text:**** st   
****Decryption:****   
  t -> s  
  l -> t

![Rules for Decryption 1](https://media.geeksforgeeks.org/wp-content/uploads/20190818175435/encryption-of-st.png)

- ****If neither of the above rules is true****: Form a rectangle with the two letters and take the letters on the horizontal opposite corner of the rectangle.

****For example:**** 

****Diagraph:**** "rq"   
****Decrypted Text:**** nt   
****Decryption:****   
  r -> n  
  q -> t

![Rules for Decryption 2](https://media.geeksforgeeks.org/wp-content/uploads/20190818175433/encryption-of-nt.png)****For example:****   
 

****Plain Text:**** "gatlmzclrqtx"  
****Decrypted Text:**** instrumentsz  
****Decryption:****   
(red)-> (green)  
  ga -> in  
  tl -> st  
  mz -> ru  
  cl -> me  
  rq -> nt  
  tx -> sz  

  
 

![Example of Decryption](https://media.geeksforgeeks.org/wp-content/uploads/20190818175428/encryption-of-instruments.png)

  
Below is an implementation of Playfair Cipher Decryption in C:   
 

C++CJavaPythonJavaScript

`import numpy as np  def to_lower_case(text):     return text.lower()  def remove_spaces(text):     return text.replace(" ", "")  def generate_key_table(key):     key = remove_spaces(to_lower_case(key))     key = key.replace('j', 'i')     key = ''.join(dict.fromkeys(key))  # Remove duplicate letters      alphabet = "abcdefghiklmnopqrstuvwxyz"  # 'j' is excluded     key_table = [c for c in key if c in alphabet]      for char in alphabet:         if char not in key_table:             key_table.append(char)      key_table = np.array(key_table).reshape(5, 5)     return key_table  def search(key_table, a, b):     if a == 'j':         a = 'i'     if b == 'j':         b = 'i'      p1 = p2 = None     for i in range(5):         for j in range(5):             if key_table[i, j] == a:                 p1 = (i, j)             elif key_table[i, j] == b:                 p2 = (i, j)     return p1, p2  def decrypt(cipher, key):     key_table = generate_key_table(key)     deciphered = []      for i in range(0, len(cipher), 2):         p1, p2 = search(key_table, cipher[i], cipher[i+1])          if p1[0] == p2[0]:             deciphered.append(key_table[p1[0], (p1[1]-1)%5])             deciphered.append(key_table[p2[0], (p2[1]-1)%5])         elif p1[1] == p2[1]:             deciphered.append(key_table[(p1[0]-1)%5, p1[1]])             deciphered.append(key_table[(p2[0]-1)%5, p2[1]])         else:             deciphered.append(key_table[p1[0], p2[1]])             deciphered.append(key_table[p2[0], p1[1]])      return ''.join(deciphered)  # Driver code if __name__ == "__main__":     key = "Monarchy"     print("Key Text:", key)      cipher = "gatlmzclrqtx"     print("Ciphertext:", cipher)      decrypted_text = decrypt(cipher, key)     print("Deciphered text:", decrypted_text)`

  
**Output**

Key text: Monarchy
Plain text: gatlmzclrqtx
Deciphered text: instrumentsz

**Advantages and Disadvantages**

****Advantages:**** 

- It is significantly harder to break since the frequency analysis technique used to break simple substitution ciphers is difficult but still can be used on (25*25) = 625 digraphs rather than 25 monographs which is difficult.  
- Frequency analysis thus requires more cipher text to crack the encryption. 

****Disadvantages:**** 

- An interesting weakness is the fact that a digraph in the ciphertext (AB) and it’s reverse (BA) will have corresponding plaintexts like UR and RU (and also ciphertext UR and RU will correspond to plaintext AB and BA, i.e. the substitution is self-inverse). That can easily be exploited with the aid of frequency analysis, if the language of the plaintext is known. 
- Another disadvantage is that playfair cipher is a [symmetric cipher](https://www.geeksforgeeks.org/traditional-symmetric-ciphers/) thus same key is used for both encryption and decryption.