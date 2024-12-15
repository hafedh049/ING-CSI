The **Data Encryption Standard (DES)** is a symmetric key encryption algorithm that was widely used in the past for secure data encryption. Although now considered insecure due to its relatively short key length, DES provides a foundational understanding of symmetric encryption.

Here is a detailed explanation of how DES works:

---

### **1. Overview**

- **Key Length**: 56 bits (plus 8 parity bits, making it 64 bits in total).
- **Block Size**: Operates on 64-bit blocks of plaintext.
- **Rounds**: Performs 16 rounds of complex transformations.

DES uses a combination of substitution (S-boxes) and permutation to achieve confusion and diffusion, which are essential properties for secure encryption.

---

### **2. Steps of DES Encryption**

#### **Step 1: Initial Permutation (IP)**

- The 64-bit plaintext block undergoes an initial permutation (IP), which rearranges the bits based on a predefined table.
- The purpose of IP is to shuffle the bits to reduce correlation between the plaintext and ciphertext.

#### **Step 2: Splitting into Two Halves**

- After the initial permutation, the 64-bit block is divided into two 32-bit halves:
    - Left half (L₀)
    - Right half (R₀)

#### **Step 3: Key Generation**

- DES uses a 56-bit key (after discarding 8 parity bits from the original 64-bit key).
- 16 subkeys, each 48 bits long, are derived from the main key using:
    - **Permutation Choice 1 (PC-1)**: Rearranges and compresses the key to 56 bits.
    - **Left Circular Shifts (LS)**: Shifts the halves of the key by 1 or 2 bits in each round.
    - **Permutation Choice 2 (PC-2)**: Further compresses the key halves to 48 bits.
- Each round uses a unique 48-bit subkey.

#### **Step 4: The Feistel Function (F-function)**

- The Feistel function operates on the right half (Rₙ) of the data block and consists of:
    1. **Expansion (E)**:
        - Expands the 32-bit Rₙ into 48 bits by duplicating and permuting bits.
    2. **Key Mixing**:
        - The expanded Rₙ is XORed with the 48-bit subkey for the round.
    3. **Substitution (S-boxes)**:
        - The result is divided into 8 groups of 6 bits each.
        - Each group is passed through a unique S-box, which maps the 6 bits to 4 bits based on predefined substitution rules.
        - The 8 S-box outputs produce a 32-bit result.
    4. **Permutation (P)**:
        - The 32-bit output of the S-boxes is permuted according to a fixed table.

#### **Step 5: Combining Lₙ and Rₙ**

- The output of the Feistel function is XORed with the left half (Lₙ), and the result becomes the new Rₙ⁺¹.
- The old Rₙ becomes the new Lₙ⁺¹.
- This process is repeated for 16 rounds, with L and R being swapped at each step.

#### **Step 6: Final Permutation (FP)**

- After 16 rounds, the two halves (L₁₆ and R₁₆) are combined and passed through the final permutation (FP), which is the inverse of the initial permutation (IP).

---

### **3. Decryption Process**

Decryption in DES is essentially the reverse of encryption:

- The same 16 subkeys are used, but in reverse order.
- The ciphertext undergoes the same steps: FP⁻¹ → 16 rounds → IP⁻¹.

---

### **4. Strengths and Weaknesses**

#### **Strengths**

- Simple and fast due to its symmetric nature.
- The Feistel structure ensures high diffusion and confusion.
- Substitution and permutation operations make brute force challenging for small datasets.

#### **Weaknesses**

- **Key Length**: The 56-bit key is too short by modern standards, making it vulnerable to brute-force attacks.
- **Block Size**: The 64-bit block size is insufficient for encrypting large amounts of data securely.
- **Known Vulnerabilities**: Techniques like differential and linear cryptanalysis can compromise DES.

---

### **5. Modern Alternatives**

Due to its weaknesses, DES has been replaced by more secure algorithms such as:

- **Triple DES (3DES)**: Applies DES three times with different keys to increase security.
- **Advanced Encryption Standard (AES)**: A block cipher with key sizes of 128, 192, or 256 bits, offering much stronger encryption.

---

### **Diagram of DES Workflow**

1. **Initial Permutation (IP)**: Input (plaintext) → Permuted Input
2. **16 Rounds**: Feistel Function (E, XOR, S-Box, P) + Subkey
3. **Final Permutation (FP)**: Combined Output → Ciphertext

---

![[Pasted image 20241210204048.png]]

![[Pasted image 20241210204117.jpg]]

The **Advanced Encryption Standard (AES)** is a widely used symmetric key encryption algorithm that replaced DES due to its superior security and efficiency. AES is highly versatile, capable of encrypting data with key sizes of 128, 192, or 256 bits. It is the standard for securing sensitive data worldwide.

---

### **1. Overview**

- **Key Lengths**: AES supports 128, 192, and 256 bits.
- **Block Size**: Fixed at 128 bits (16 bytes).
- **Rounds**:
    - **10 rounds** for a 128-bit key.
    - **12 rounds** for a 192-bit key.
    - **14 rounds** for a 256-bit key.

AES is based on a **substitution-permutation network** (SPN), using multiple layers of transformations for encryption.

---

### **2. Steps of AES Encryption**

#### **Step 1: Key Expansion**

- The input key is expanded into multiple round keys using the **Rijndael key schedule**.
- Each round uses a unique round key derived from the original key.

#### **Step 2: Initial Round**

1. **AddRoundKey**:
    - The 128-bit plaintext block is XORed with the first round key.

#### **Step 3: Main Rounds**

Each main round consists of four operations:

1. **SubBytes (Substitution)**:
    
    - Each byte in the state (128-bit block) is replaced with a byte from the **S-box** (Substitution Box), which provides non-linearity and resistance against cryptanalysis.
2. **ShiftRows (Permutation)**:
    
    - The rows of the state matrix are shifted:
        - Row 0: No shift.
        - Row 1: One byte to the left.
        - Row 2: Two bytes to the left.
        - Row 3: Three bytes to the left.
    - This introduces diffusion by mixing the data across the block.
3. **MixColumns**:
    
    - Each column of the state is treated as a polynomial and multiplied with a fixed polynomial in a Galois Field (GF(2⁸)).
    - This operation mixes the bytes within each column, adding further diffusion.
4. **AddRoundKey**:
    
    - The state matrix is XORed with the current round key.

#### **Step 4: Final Round**

The final round is similar to the main rounds but skips the **MixColumns** step:

1. SubBytes
2. ShiftRows
3. AddRoundKey

#### **Step 5: Ciphertext**

The output of the final round is the 128-bit ciphertext.

---

### **3. AES Decryption**

Decryption is essentially the reverse of encryption:

- The inverse of each transformation (InvSubBytes, InvShiftRows, InvMixColumns) is applied.
- The same round keys are used, but in reverse order.

---

### **4. Strengths of AES**

1. **Security**:
    
    - AES is resistant to brute-force attacks due to its large key sizes.
    - The substitution-permutation design makes it resistant to linear and differential cryptanalysis.
2. **Efficiency**:
    
    - AES is computationally efficient and optimized for both hardware and software implementations.
    - Its fixed block size simplifies integration into protocols like SSL/TLS.
3. **Flexibility**:
    
    - Supports multiple key sizes for varying security requirements.

---

### **5. Differences Between DES and AES**

|Feature|DES|AES|
|---|---|---|
|Block Size|64 bits|128 bits|
|Key Length|56 bits|128, 192, or 256 bits|
|Rounds|16 rounds|10, 12, or 14 rounds|
|Structure|Feistel Network|Substitution-Permutation Network|
|Security|Vulnerable to brute-force|Secure against modern attacks|

---

### **6. Applications of AES**

- Securing online communications (TLS/SSL).
- Protecting data at rest (disk encryption, databases).
- Encrypting wireless communications (WPA2/WPA3).
- Used in standards like FIPS 197 and by the US government.

