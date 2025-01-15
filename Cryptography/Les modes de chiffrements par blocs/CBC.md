### **Cipher Block Chaining (CBC) Algorithm**

The **Cipher Block Chaining (CBC)** mode is one of the most widely used and secure encryption modes when used with proper implementation. It introduces dependency between plaintext blocks by incorporating the ciphertext of the previous block into the encryption of the current block.

![[Pasted image 20250114153429.png]]

---

### **How CBC Works**

1. **Initialization**:
    
    - A random **Initialization Vector (IV)** is used for the first block. The IV ensures that identical plaintexts encrypted with the same key produce different ciphertexts.
2. **Encryption**:
    
    - Each block of plaintext is XORed with the ciphertext of the previous block before being encrypted.
    - For the first block, the IV is used instead of a previous ciphertext block.
    
    The encryption formula for block $i$ is:
    
    $C_i = E_K(P_i \oplus C_{i-1})$
    
    where:
    
    - $E_K$ is the encryption function using key $K$,
    - $P_i$ is the plaintext block,
    - $C_{i-1}$ is the previous ciphertext block (or $IV$ for the first block).
3. **Decryption**:
    
    - Each ciphertext block is decrypted first, then XORed with the previous ciphertext block (or $IV$ for the first block) to retrieve the plaintext.
    - The decryption formula for block ii is: $P_i = D_K(C_i) \oplus C_{i-1}$ .

---

### **Step-by-Step Example**

Let’s encrypt three blocks of plaintext $P_1$, $P_2$, and $P_3$:

1. **Initialization**:
    
    - Choose an $IV$ (e.g., $IV$ = $10101010$ in binary).
2. **Block 1 Encryption**:
    
    - XOR $P_1$ with $IV$: $P_1 \oplus IV$
    - Encrypt the result using the key $K$: $C_1 = E_K(P_1 \oplus IV)$
3. **Block 2 Encryption**:
    
    - XOR $P_2$ with $C_1$: $P_2 \oplus C_1$
    - Encrypt the result using $K$: $C_2 = E_K(P_2 \oplus C_1)$
4. **Block 3 Encryption**:
    
    - XOR $P_3$ with $C_2$: $P_3 \oplus C_2$
    - Encrypt the result using $K$: $C_3 = E_K(P_3 \oplus C_2)$

**Decryption**:

- Decrypt $C_i$, then XOR with $C_{i-1}$ to retrieve $P_i$: 
- $P_1 = D_K(C_1) \oplus IV$ 
- $P_2 = D_K(C_2) \oplus C_1$ 
- $P_3 = D_K(C_3) \oplus C_2$

---

### **Advantages of CBC**

1. **High Security**:
    
    - Patterns in the plaintext are obscured due to the chaining of ciphertext blocks.
    - Identical plaintext blocks produce different ciphertext blocks if different IVs are used.
2. **Error Propagation**:
    
    - An error in one block affects only two blocks during decryption, limiting the propagation.

---

### **Disadvantages of CBC**

1. **Sequential Processing**:
    
    - Encryption and decryption cannot be parallelized due to the dependency on the previous block.
    - Slower than parallelizable modes like CTR or GCM.
2. **IV Management**:
    
    - Requires a unique, random IV for every encryption to maintain security.
    - The IV must be transmitted securely (often in plaintext with the ciphertext).
3. **Error Propagation**:
    
    - While limited to two blocks, a single-bit error in the ciphertext can corrupt the corresponding plaintext block and affect the next one.

---

### **Illustrative Example**

#### Input Data:

Plaintext: $HELLOHELLO$  
Block Size: 5 characters/block  
IV: Random (e.g., $10101$)

#### Encryption:

1. First Block:
    
    - XOR $P_1 = HELLO$ with $IV$.
    - Encrypt the result to get $C_1$.
2. Second Block:
    
    - XOR $P_2 = HELLO$ with $C_1$.
    - Encrypt the result to get $C_2$.

#### Observations:

- Even though both blocks contain the same plaintext ($HELLO$), the ciphertext differs due to chaining.

---

### **Visual Representation**

If CBC is used to encrypt an image, the encrypted output appears random with no visible structure, unlike ECB.

---

### **When to Use CBC**

- **File encryption**: Protects data at rest.
- **Data transmission**: Ensures confidentiality over a network.
- **Databases**: Encrypts structured data without revealing patterns.

---

### **Conclusion**

CBC is a robust and widely used encryption mode suitable for many applications. However, care must be taken to handle the IV properly and ensure unique values for every encryption operation to maintain security. For modern systems, authenticated modes like GCM are often preferred to add integrity and authentication.