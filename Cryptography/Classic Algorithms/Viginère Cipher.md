
# Vigenère Cipher

Last Updated : 18 Jul, 2024

Vigenere Cipher is a method of encrypting alphabetic text. It uses a simple form of [polyalphabetic substitution](https://en.wikipedia.org/wiki/Polyalphabetic_cipher). A polyalphabetic cipher is any cipher based on substitution, using multiple substitution alphabets. The encryption of the original text is done using the [__Vigenère square or Vigenère table__](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher#/media/File:Vigen%C3%A8re_square_shading.svg).

- The table consists of the alphabets written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible [Caesar Ciphers](https://www.geeksforgeeks.org/caesar-cipher/).
- At different points in the encryption process, the cipher uses a different alphabet from one of the rows.
- The alphabet used at each point depends on a repeating keyword.

****Example:**** 

Input : Plaintext :   GEEKSFORGEEKS  
       Keyword :  AYUSH  
Output : Ciphertext :  GCYCZFMLYLEIM  
For generating key, the given keyword is repeated  
in a circular manner until it matches the length of   
the plain text.  
The keyword "AYUSH" generates the key "AYUSHAYUSHAYU"  
The plain text is then encrypted using the process   
explained below.

****Encryption:****

The first letter of the plaintext, G is paired with A, the first letter of the key. So use row G and column A of the Vigenère square, namely G. Similarly, for the second letter of the plaintext, the second letter of the key is used, the letter at row E, and column Y is C. The rest of the plaintext is enciphered in a similar fashion. 

****Table to encrypt – Geeks****

![Vigenère_square_shading](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Vigen%C3%A8re_square_shading.png)

****Decryption:****  
Decryption is performed by going to the row in the table corresponding to the key, finding the position of the ciphertext letter in this row, and then using the column’s label as the plaintext. For example, in row A (from AYUSH), the ciphertext G appears in column G, which is the first plaintext letter. Next, we go to row Y (from AYUSH), locate the ciphertext C which is found in column E, thus E is the second plaintext letter.

A more ****easy implementation**** could be to visualize Vigenère algebraically by converting [A-Z] into numbers [0–25]. 

****Encryption****  
The plaintext(P) and key(K) are added modulo 26.  
Ei = (Pi + Ki) mod 26  
  
****Decryption****  
Di = (Ei - Ki) mod 26

****Note:**** Di denotes the offset of the i-th character of the plaintext. Like offset of ****A**** is 0 and of ****B**** is 1 and so on.

****Time Complexity : O(n)****, where n is the length of the string(here str).

****Space Complexity :O(n)****, here n is the length of the string(here str).