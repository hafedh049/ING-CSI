### **RSA Algorithm: How It Works**

RSA (Rivest–Shamir–Adleman) is a widely used asymmetric encryption algorithm based on the mathematical properties of prime numbers. It is used for secure data transmission, digital signatures, and encryption.

![[Pasted image 20250114161256.png]]

---
### **Key Components of RSA**

1. **Public Key**: Used for encryption and shared openly.
2. **Private Key**: Used for decryption and kept secret.
3. **Modulus ($n$)**: A product of two large prime numbers, $p$ and $q$.
4. **Public Exponent (e)**: A number used for encryption, chosen such that it is relatively prime to $\phi(n)$ (explained below).
5. **Private Exponent ($d$)**: A number used for decryption, calculated from $e$ and $\phi(n)$.

---

### **Steps in RSA**

#### 1. **Key Generation**

- **Step 1**: Choose two large prime numbers, $p$ and $q$.
    
- **Step 2**: Compute the modulus:
    
    $n = p \times q$
    
- **Step 3**: Compute Euler's totient function $\phi(n)$:
    
    $\phi(n) = (p-1) \times (q-1)$

- **Step 4**: Choose the public exponent $e$:
    
    - $e$ must satisfy $1 < e < \phi(n)$,
    - $e$ must be coprime with $\phi(n)$ (i.e., $\text{gcd}(e, \phi(n)) = 1$).

- **Step 5**: Compute the private exponent $d$:
    
	 $d \equiv e^{-1} \pmod{\phi(n)}$
	- $d$ is the modular multiplicative inverse of $e$ modulo $\phi(n)$.

- **Public Key**: $(e, n)$
    
- **Private Key**: $(d, n)$
    
---
#### 2. **Encryption**

- The sender encrypts the plaintext $M$ (converted into an integer such that $0 \leq M < n$) using the recipient's public key $(e, n)$: $C = M^e \mod n$
- $C$ is the ciphertext.

---

#### 3. **Decryption**

- The recipient decrypts the ciphertext $C$ using their private key $(d, n)$: $M = C^d \mod n$
- $M$ is the original plaintext.

---
### **Example: Step-by-Step**

#### 1. **Key Generation**

- Choose two prime numbers: $p = 61, q = 53$.
    
- Compute $n$:
    
    $n = 61 \times 53 = 3233$
    
- Compute $\phi(n)$:
    
    $\phi(n) = (61 - 1) \times (53 - 1) = 60 \times 52 = 3120$

- Choose $e$:
    
    - Let $e = 17$, which satisfies $1 < e < 3120$ and $\text{gcd}(17, 3120) = 1$.
- Compute $d$:
    
    - Find $d$ such that $d \times e \equiv 1 \pmod{3120}$.
    - Using the Extended Euclidean Algorithm: $d = 2753$
    
- **Public Key**: $(e, n) = (17, 3233)$
    
- **Private Key**: $(d, n) = (2753, 3233)$

---
#### 2. **Encryption**

- Plaintext message $M = 65$ (convert text to numeric form, e.g., ASCII).
- Compute ciphertext $C$: $C = M^e \mod n = 65^{17} \mod 3233$
- Simplifying: $C = 2790$

---

#### 3. **Decryption**

- Ciphertext $C = 2790$.
- Compute plaintext $M$: $M = C^d \mod n = 2790^{2753} \mod 3233$
- Simplifying: $M = 65$
- Convert $M$ back to text: $A$.

---

### **Advantages of RSA**

1. **Security**: Based on the difficulty of factoring large numbers.
2. **Asymmetric**: No need to share private keys.
3. **Versatile**: Used for encryption, digital signatures, and key exchange.

---

### **Disadvantages of RSA**

1. **Performance**: Slower than symmetric encryption algorithms (e.g., AES).
2. **Key Management**: Requires secure storage of private keys.
3. **Key Size**: Requires large key sizes (e.g., 2048 or 4096 bits) for strong security.

---

### **Applications of RSA**

1. **Secure Communications**: Encrypting emails, messages, and sensitive data.
2. **Digital Signatures**: Verifying the authenticity of messages or documents.
3. **Key Exchange**: Exchanging keys securely in SSL/TLS protocols.

---

### **Conclusion**

The RSA algorithm is a cornerstone of modern cryptography. Its asymmetric nature provides security for a wide range of applications. However, proper key management and sufficient key lengths are crucial to ensure its robustness.