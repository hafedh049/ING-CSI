### **Cipher Feedback (CFB) Mode**

The **Cipher Feedback (CFB)** mode is a block cipher mode of operation that transforms a block cipher into a self-synchronizing stream cipher. It operates by encrypting feedback data from the previous ciphertext block (or initialization vector for the first block) to produce a keystream, which is then XORed with the plaintext to generate the ciphertext.

![[Pasted image 20250114154019.png]]

---

### **How CFB Works**

1. **Initialization**:
    
    - Requires an **Initialization Vector (IV)** of the same size as the block cipher.
    - The IV ensures that even if the same plaintext is encrypted multiple times with the same key, the ciphertext will differ.
2. **Encryption**:
    
    - The cipher operates on a feedback mechanism, where the previous ciphertext block (or IV for the first block) is encrypted to generate the keystream.
    - The plaintext block is XORed with the keystream to produce the ciphertext block.
    
    The encryption formula for block ii is:
    
    $C_i = P_i \oplus E_K(F_{i-1})$
    
    Where:
    
    - $E_K$ is the encryption function with key $K$,
    - $P_i$ is the plaintext block,
    - $F_{i-1}$ is the feedback value ($IV$ for the first block, or $C_{i-1}$ for subsequent blocks),
    - $C_i$ is the ciphertext block.
3. **Decryption**:
    
    - Decrypting ciphertext involves the same process as encryption since XOR is a symmetric operation: $P_i = C_i \oplus E_K(F_{i-1})$

---

### **Step-by-Step Example**

#### Parameters:

- Block size: 128 bits.
- Key: $K$.
- IV: $IV$.

#### Encryption Process:

1. **First Block**:
    
    - Encrypt the $IV$: $E_K(IV)$.
    - XOR the encrypted $IV$ with the plaintext block $P_1$: $C_1 = P_1 \oplus E_K(IV)$
2. **Second Block**:
    
    - Use $C_1$ as feedback and encrypt it: $E_K(C_1)$.
    - XOR with the next plaintext block $P_2$ : $C_2 = P_2 \oplus E_K(C_1)$
3. **Subsequent Blocks**:
    
    - Repeat the process, using the previous ciphertext block as feedback.

#### Decryption Process:

- Decryption uses the same encryption function $E_K$, as XOR is symmetric:
    - For each block: $P_i = C_i \oplus E_K(F_{i-1})$

---

### **Advantages of CFB**

1. **Stream Cipher Transformation**:
    
    - Converts a block cipher into a stream cipher, which is useful for encrypting data streams of arbitrary lengths.
2. **Self-Synchronizing**:
    
    - If some ciphertext is lost, decryption can resume after a few blocks, as the feedback will realign.
3. **No Padding Required**:
    
    - Since the encryption operates on bits or bytes, there’s no need to pad plaintext to match the block size.

---

### **Disadvantages of CFB**

1. **Sequential Processing**:
    
    - Encryption and decryption cannot be parallelized due to the dependency on the previous block.
2. **Error Propagation**:
    
    - A single-bit error in the ciphertext affects the decryption of the current and next block.

---

### **Illustrative Example**

#### Input Data:

- **Plaintext**: $HELLOHELLO$.
- **Block Size**: 5 characters/block.
- **IV**: Random (e.g., $IV = 10101$).

#### Process:

1. Encrypt the IV to generate a keystream: $E_K(IV)$.
2. XOR the keystream with the first plaintext block to produce the first ciphertext block.
3. Use the ciphertext block as feedback for the next encryption step.

---

### **When to Use CFB**

- **Data streams**: Ideal for encrypting real-time data like network traffic or audio streams.
- **Partial encryption**: Useful when only a portion of the plaintext needs encryption.
- **Hardware and software**: Often used in software implementations due to its ability to encrypt data without padding.

---

### **Comparison to Other Modes**

- Unlike ECB and CBC, CFB operates more like a stream cipher, providing flexibility for non-block-sized data.
- Compared to CTR, CFB does not support parallelization but offers self-synchronizing capabilities.

---

### **Conclusion**

CFB is a versatile encryption mode, especially suited for stream-based encryption and scenarios where block sizes vary. However, its sequential nature can be a bottleneck in performance-critical applications. Proper IV handling is crucial to maintaining security.