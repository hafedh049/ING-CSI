### **Hill Cipher Overview**

The **Hill Cipher** is a **polygraphic substitution cipher** based on **linear algebra**. It encrypts blocks of text using **matrix multiplication** over a **modular arithmetic system** (typically mod 26 for English alphabets). It operates on groups of characters rather than individual ones, which makes it resistant to frequency analysis.

---

### **How Hill Cipher Works**

1. **Choose a Key Matrix (K):**  
   This is an `n x n` matrix (for **n-letter blocks**).  
   Example:
   ```python
   K = | 6  24  1 |
       | 13 16 10 |
       | 20 17 15 |
   ```

2. **Plaintext Block (P):**  
   Divide the plaintext into **blocks of size n**. If the plaintext length isn’t divisible by n, **pad it** with dummy letters (like 'X').  

   Example plaintext: `"ACT"`  
   Convert letters to numbers:
   ```python
   A = 0, C = 2, T = 19
   P = [0, 2, 19]
   ```

3. **Encryption Formula:**  
   Use matrix multiplication to generate the cipher text:
   ```python
   C = (K × P) mod 26
   ```
   Where:
   - `C` is the cipher text block
   - `K` is the key matrix
   - `P` is the plaintext block

4. **Decryption Formula:**  
   To decrypt, you need the **inverse of the key matrix (K⁻¹)**, such that:
   ```python
   P = (K⁻¹ × C) mod 26
   ```

---

### **Hill Cipher Example**

Let's walk through encrypting and decrypting the word **"ACT"** with the 3x3 key matrix shown above.

#### **Key Matrix (K)**  
```python
K = | 6  24  1 |
    | 13 16 10 |
    | 20 17 15 |
```

#### **Plaintext Block (P)**  
Convert **"ACT"** into numbers:
```python
A = 0, C = 2, T = 19
P = [0, 2, 19]
```

#### **Step 1: Encryption**

Multiply the key matrix with the plaintext block:
```python
C = K × P mod 26
  = | 6  24  1 |   ×   | 0 |
    | 13 16 10 |       | 2 |
    | 20 17 15 |       | 19 |

  = | (6*0 + 24*2 + 1*19) |
    | (13*0 + 16*2 + 10*19) |
    | (20*0 + 17*2 + 15*19) |

  = | 67 |
    | 216 |
    | 314 |

Now, take **mod 26**:
```
67 mod 26 = 15  
216 mod 26 = 8  
314 mod 26 = 2
```python
So, the cipher text block is:
```
C = [15, 8, 2]
```python
Convert these numbers back to letters:
```
15 = P, 8 = I, 2 = C
```python

Thus, the encrypted message for **"ACT"** is `"PIC"`.

---

#### **Step 2: Decryption**

To decrypt, we need the **inverse of the key matrix**. This is a bit involved, as we need to:

1. **Find the determinant** of the key matrix.
2. **Find the modular inverse** of the determinant mod 26.
3. **Calculate the adjugate matrix** and multiply by the inverse of the determinant mod 26.

After calculating, assume the inverse matrix \( K^{-1} \) is:
```

K⁻¹ =  | 8  5 10 |
      | 21 8 21 |
      | 21 12 8 |
```python

Now, multiply the inverse matrix with the cipher text block **[15, 8, 2]** to get back the original plaintext block **[0, 2, 19]**, which corresponds to **"ACT"**.

---

### **Code Implementation of Hill Cipher in Python**

```python
import numpy as np

def hill_cipher_encrypt(plaintext, key_matrix):
    # Convert plaintext to numbers (A=0, B=1, ..., Z=25)
    plaintext_nums = [ord(char) - ord('A') for char in plaintext]

    # Perform matrix multiplication and mod 26
    cipher_nums = np.dot(key_matrix, plaintext_nums) % 26

    # Convert numbers back to letters
    cipher_text = ''.join(chr(num + ord('A')) for num in cipher_nums)
    return cipher_text

def hill_cipher_decrypt(cipher_text, key_matrix):
    # Convert cipher text to numbers
    cipher_nums = [ord(char) - ord('A') for char in cipher_text]

    # Calculate the inverse matrix mod 26
    det = int(round(np.linalg.det(key_matrix)))
    det_inv = pow(det, -1, 26)  # Modular inverse of determinant

    key_matrix_inv = (
        det_inv * np.round(det * np.linalg.inv(key_matrix)).astype(int) % 26
    )

    # Perform matrix multiplication and mod 26
    plaintext_nums = np.dot(key_matrix_inv, cipher_nums) % 26

    # Convert numbers back to letters
    plaintext = ''.join(chr(int(num) + ord('A')) for num in plaintext_nums)
    return plaintext

# Test the Hill Cipher
key_matrix = np.array([[6, 24, 1], [13, 16, 10], [20, 17, 15]])
plaintext = "ACT"

cipher_text = hill_cipher_encrypt(plaintext, key_matrix)
print("Encrypted:", cipher_text)  # Output: PIC

decrypted_text = hill_cipher_decrypt(cipher_text, key_matrix)
print("Decrypted:", decrypted_text)  # Output: ACT
```

---

### **Summary**

- **Hill Cipher** encrypts blocks of text using matrix multiplication.
- Decryption requires finding the **inverse of the key matrix**, which can be tricky.
- It's stronger than simple substitution ciphers because it encrypts multiple letters at once, making it resistant to frequency analysis.

Let me know if you need further clarification!