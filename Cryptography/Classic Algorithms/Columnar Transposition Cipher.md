
Given a plain-text message and a numeric key, cipher/de-cipher the given text using Columnar Transposition Cipher The Columnar Transposition Cipher is a form of transposition cipher just like [Rail Fence Cipher](https://www.geeksforgeeks.org/rail-fence-cipher-encryption-decryption/). Columnar Transposition involves writing the plaintext out in rows, and then reading the ciphertext off in columns one by one.

****Examples:****

****Encryption****
Input : Geeks for Geeks
Key = HACK
Output : e  kefGsGsrekoe_
****Decryption****
Input : e  kefGsGsrekoe_
Key = HACK
Output : Geeks for Geeks 
****Encryption****
Input :  Geeks on work
Key = HACK
Output : e w_eoo_Gs kknr_
****Decryption****
Input : e w_eoo_Gs kknr_
Key = HACK
Output : Geeks on work

****Encryption****

In a transposition cipher, the order of the alphabets is re-arranged to obtain the cipher-text.

1. The message is written out in rows of a fixed length, and then read out again column by column, and the columns are chosen in some scrambled order.
2. Width of the rows and the permutation of the columns are usually defined by a keyword.
3. For example, the word HACK is of length 4 (so the rows are of length 4), and the permutation is defined by the alphabetical order of the letters in the keyword. In this case, the order would be “3 1 2 4”.
4. Any spare spaces are filled with nulls or left blank or placed by a character (Example: _).
5. Finally, the message is read off in columns, in the order specified by the keyword.

![columnar-transposition-cipher](https://media.geeksforgeeks.org/wp-content/uploads/columnar-transposition-cipher1.png)

****Decryption****

1. To decipher it, the recipient has to work out the column lengths by dividing the message length by the key length.
2. Then, write the message out in columns again, then re-order the columns by reforming the key word.
