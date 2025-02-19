
file:///C:/Users/Hafedh%20GUENICHI/Downloads/1.pdf

# Exercice I

1. **Avec les ordinateurs quantiques, la cryptographie asymétrique sera vulnérable** car les algorithmes comme RSA seront facilement résolus grâce à l'algorithme de Shor.
    
2. **Cryptographie symétrique** : même clé pour chiffrer et déchiffrer. **Problème majeur** : gestion et échange sécurisé de la clé secrète.
    
3. **QKD** signifie **Quantum Key Distribution**, une méthode utilisant la mécanique quantique pour partager des clés secrètes en toute sécurité.

# Exercice II

> [!tip]
> Soient les deux **kets** indépendants
> $$ |x\rangle = \begin{pmatrix} \frac{1}{2} \\ \frac{\sqrt{3}}{2} e^{i\theta} \end{pmatrix}$$ $$ |y\rangle = \begin{pmatrix} \frac{\sqrt{3}}{2} \\ - \frac{e^{i\theta}}{2} \end{pmatrix}$$ 
  
#### 1. Montrer qu’ils représentent des qubits dans la base standard de calcul, et qu’ils sont orthogonaux.

#### 1. Déterminer les bra des deux kets

#### 1. Calculer les produits brakets : < 𝑥|𝑦 > 𝑒𝑡 < 𝑦|𝑥 >. Que peut-on conclure ?

#### 1. En déduire que c’est une base orthonormée des qubits

#### 1. Quelle est la probabilité d’obtenir |1 > si on effectue une mesure quantique de |𝑥 > dans la base standard de calcul. Justifier votre réponse.

# Exercice III

#### **1) Choosing the Most Suitable Raw Key for Alice**

- **Question:** Which of the chains is the most suitable to represent Alice’s raw key? Justify your answer.
    
- **Answer:**  
    The most suitable chain is **Chaine 3 (00110100110101011100)**.
    
- **Justification:**
    
    - **Binary:** The key must only contain **0s and 1s**, as quantum key distribution (QKD) relies on binary polarization states.
    - **Equilibrate:** The key should have a roughly **equal distribution of 0s and 1s** to maximize security and randomness.
    - **Random:** The key should not follow a predictable pattern to ensure it is **unpredictable to an eavesdropper**.
- **Why the other chains are not suitable:**
    
    - **Chaine 1 (11001100110011001100):**
        - It is binary but **not random**—it follows a repeating pattern (1100).
    - **Chaine 2 (10110000100101000000):**
        - It is binary but **not equilibrate**—there are **too many 0s compared to 1s**, making it unbalanced.
    - **Chaine 4 (00121010201130101233):**
        - It **contains non-binary digits (2 and 3)**, which are **invalid** for QKD.

#### **1c) Encoding Chaine 3 in Polarized Photons Using the B92 Protocol**

Using the B92 protocol:

- 00 → **↘ (Diagonal polarization)**
- 11 → **↔ (Horizontal polarization)**

Encoding **Chaine 3 (00110100110101011100)**:

↘ ↘ ↔ ↔ ↘ ↔ ↘ ↘ ↔ ↔ ↘ ↔ ↕ ↘ ↔ ↕ ↔ ↘ ↘ ↘\text{↘ ↘ ↔ ↔ ↘ ↔ ↘ ↘ ↔ ↔ ↘ ↔ ↕ ↘ ↔ ↕ ↔ ↘ ↘ ↘}

---
#### **2) Photons Polarized by Alice (BB84 Protocol)**

- **Question:** Provide the sequence of polarized photons sent by Alice if her encoding bases are:  
    `× + + × + × × + × + + + + × × + × × × +`
    
- **Answer:** Using the BB84 protocol:
    
    - **+ Basis (Rectilinear):**
        - `0` → **↔ (Horizontal)**
        - `1` → **↕ (Vertical)**
    - **× Basis (Diagonal):**
        - `0` → **↘ (Diagonal right)**
        - `1` → **↙ (Diagonal left)**
    
    If Alice’s raw key is **Chaine 3 (00110100110101011100)**:
    
    - Mapping each bit according to the bases:

|Bit|Basis (+ or ×)|Photon Polarization|
|---|---|---|
|0|×|↘|
|0|+|↔|
|1|+|↕|
|1|×|↙|
|0|+|↔|
|1|×|↙|
|0|×|↘|
|0|+|↔|
|1|×|↙|
|0|+|↔|
|0|+|↔|
|1|+|↕|
|1|×|↙|
|0|×|↘|
|1|+|↕|
|0|×|↘|
|1|×|↙|
|1|×|↙|
|0|+|↔|

- **Final polarization sequence sent by Alice:**  
    **↘ ↔ ↕ ↙ ↔ ↙ ↘ ↔ ↙ ↔ ↔ ↕ ↙ ↘ ↕ ↘ ↙ ↙ ↔**

---

#### **3) Bob’s Possible Key After Measurement**

- **Question:** What are Bob’s possible bits after measurement if his bases are:  
    `× × + × + + + × + × × + × × × + + + × +`
    
- **Answer:**
    
    - If **Bob and Alice use the same basis**, he will correctly measure Alice’s bit.
    - If **Bob and Alice use different bases**, his measurement is random (50% chance of being correct).

|Bit|Alice’s Basis|Photon|Bob’s Basis|Bob’s Bit (if same basis)|
|---|---|---|---|---|
|0|×|↘|×|0|
|0|+|↔|×|? (random)|
|1|+|↕|+|1|
|1|×|↙|×|1|
|0|+|↔|+|0|
|1|×|↙|+|? (random)|
|0|×|↘|+|? (random)|
|0|+|↔|×|? (random)|
|1|×|↙|+|? (random)|
|0|+|↔|×|? (random)|
|0|+|↔|+|0|
|1|+|↕|+|1|
|1|×|↙|×|1|
|0|×|↘|×|0|
|1|+|↕|+|1|
|0|×|↘|+|? (random)|
|1|×|↙|+|? (random)|
|1|×|↙|×|1|
|0|+|↔|+|0|

- **Bob’s measured key (with unknown bits replaced by `?`):**  
    `0 ? 1 1 0 ? ? ? ? ? 0 1 1 0 1 ? ? 1 0`

---

#### **3c) Size of the Sifted Keys**

- **Question:** What is the size of the sifted keys?
    
- **Answer:**  
    The sifted key only contains bits where **Alice and Bob used the same basis**. From the above table, the valid key bits are:
    
    **Sifted Key:** `0 1 1 0 0 1 1 0 1 1 0`  
    **Size:** **11 bits**
    

---

#### **4) Quantum Bit Error Rate (QBER) Calculation**

- **Question:** Given the sifted keys:
    
    - **Alice’s sifted key:** `0 1 1 0 1 0 0 1 1 0 1 0`
    - **Bob’s sifted key:** `0 0 1 0 1 1 0 1 1 0 0 0`
    
    Compute the QBER using comparison at positions **2, 3, 5, 8, and 12**.
    
- **Answer:**  
    Errors occur at positions where **Alice and Bob’s bits differ**:
    
    - Position 2: `1 ≠ 0` **(Error)**
    - Position 3: `1 = 1` **(Correct)**
    - Position 5: `1 = 1` **(Correct)**
    - Position 8: `1 = 1` **(Correct)**
    - Position 12: `0 ≠ 0` **(Correct)**
    
    **Number of errors = 1**  
    **Total compared positions = 5**
    
    **QBER = (Number of errors) / (Total compared positions) = 1/5 = 20%**
    

---

#### **4b) Distilled Keys**

- **Question:** Deduce the final distilled key.
    
- **Answer:**  
    Since the error rate is **within an acceptable range**, the **distilled keys** are obtained by **removing error bits** from the sifted keys.
    
    **Final distilled key:**
    
    - Alice: `0 1 1 0 1 0 1 1 0 1`
    - Bob: `0 1 1 0 1 0 1 1 0 1`

---

#### **4c) Continuation of the Protocol**

- **Question:** Will the protocol continue with a **threshold of 25%**? Justify.
    
- **Answer:**
    
    - The computed QBER **(20%)** is **less than the threshold (25%)**, meaning the protocol **can continue**.
    - If the QBER had been **greater than 25%**, the protocol would have been **terminated** due to excessive noise or eavesdropping.

---

#### **4e) Maximum QBER Allowed**

- **Question:** Find the **maximum QBER** beyond which the protocol **cannot continue**.
    
- **Answer:**  
    The protocol **stops if QBER > 25%**. Thus, the **maximum allowed QBER** is **25%**.
    

---

#### **4f) Error Correction using XOR**

- **Question:** Apply XOR correction at positions **(2,3) and (4,6)**. What is the final key called?
    
- **Answer:**
    
    - **XOR Correction:**
        - Position **(2,3):** `1 XOR 1 = 0`
        - Position **(4,6):** `0 XOR 1 = 1`
    
    **Corrected key:**  
    **`0 0 0 0 1 1 1 1 0 1`**
    
    - This is called the **"Privacy-Amplified Key"**—the final secure key after error correction and privacy amplification.