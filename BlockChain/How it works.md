### **Encryption in Blockchain: Overview**

In blockchain, encryption ensures **data security and integrity** through a combination of **hashing** and **asymmetric cryptography**.

---

### 1. **Key Concepts**

- **Hashing**: A one-way function that converts data into a fixed-size string of characters. Example: **SHA-256** in Bitcoin.
- **Public Key Cryptography (Asymmetric Encryption)**: Uses **two keys**:  
  - **Private Key**: Known only to the owner; used for signing transactions.  
  - **Public Key**: Shared with others; used for verification.

---

### 2. **Encryption Algorithms Used**

1. **SHA-256 (Secure Hash Algorithm-256)**:  
   - Used to generate unique **hashes** for transactions and blocks.
   - Example: Bitcoin uses SHA-256 to ensure block integrity.

2. **Elliptic Curve Digital Signature Algorithm (ECDSA)**:  
   - Used for **signing and verifying transactions** in Bitcoin and Ethereum.
   - It ensures that only the owner of a private key can authorize a transaction.

---

### 3. **How a Transaction is Secured?**
1. **Signing**: 
   - The sender (Alice) signs the transaction using her **private key**.
2. **Verification**: 
   - The network verifies the signature with Alice’s **public key** to ensure it’s valid and wasn’t tampered with.

---

### 4. **Python Code Example for a Transaction**

Below is a **simplified Python example** of creating, signing, and verifying a blockchain-like transaction using **ECDSA** and **SHA-256** for hashing.

```python
import hashlib
from ecdsa import SigningKey, VerifyingKey, SECP256k1

# 1. Create a Transaction
transaction = "Alice sends 5 BTC to Bob"

# 2. Hash the Transaction using SHA-256
def hash_transaction(data):
    return hashlib.sha256(data.encode()).hexdigest()

transaction_hash = hash_transaction(transaction)
print(f"Transaction Hash: {transaction_hash}")

# 3. Generate ECDSA Key Pair (Private & Public Key)
private_key = SigningKey.generate(curve=SECP256k1)  # Private key
public_key = private_key.verifying_key              # Corresponding public key

# 4. Sign the Transaction Hash with the Private Key
signature = private_key.sign(transaction_hash.encode())
print(f"Signature: {signature.hex()}")

# 5. Verify the Signature using the Public Key
def verify_signature(public_key, data, signature):
    try:
        public_key.verify(signature, data.encode())
        print("Signature is valid! Transaction is authentic.")
    except:
        print("Invalid signature! Transaction is rejected.")

# 6. Verify the Transaction
verify_signature(public_key, transaction_hash, signature)
```

---

### 5. **Explanation of the Code**
1. **Transaction Creation**: We create a simple transaction like "Alice sends 5 BTC to Bob."
2. **Hashing**: The transaction data is hashed using **SHA-256**.
3. **Key Generation**: We generate an **ECDSA key pair** (private and public key).
4. **Signing**: The transaction hash is signed with the **private key**.
5. **Verification**: We verify the signature using the **public key** to ensure the transaction's authenticity.

---

### 6. **Conclusion**
This code demonstrates the core idea of **asymmetric cryptography** and **hashing** in blockchain transactions. While real blockchain implementations are more complex, this example captures the essentials: **signing, hashing, and verification**.