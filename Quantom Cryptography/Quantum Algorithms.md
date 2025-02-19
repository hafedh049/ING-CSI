### ✨ **BB84 Protocol**: The Pioneer of Quantum Key Distribution ✨

BB84, born in 1984 from the minds of Bennett and Brassard, was a groundbreaking protocol that laid the foundation for **secure quantum communication**. This protocol uses the fascinating properties of **quantum states of light** (usually photons) to create a secure cryptographic key between two parties, Alice and Bob, while ensuring that any eavesdropping attempt would be detected by quantum nature itself.

#### **How BB84 Works:**

The magic of BB84 rests on the principle that **measuring a quantum state disturbs it**, making it impossible for an eavesdropper to extract information without leaving a trace.

1. **Photon Polarization States**: BB84 elegantly employs **two sets of bases** for encoding information:
    
    - **Rectilinear basis**: States |0⟩ and |1⟩ (aligned along horizontal and vertical axes)
    - **Diagonal basis**: States |+⟩ and |−⟩ (at ±45°)
    
    This gives us **four possible states** that can represent a bit of information:
    
    - **|0⟩, |1⟩** (rectilinear)
    - **|+⟩, |−⟩** (diagonal)
2. **Key Generation Process**:
    
    - **Step 1**: Alice sends photons prepared in one of the four possible states.
    - **Step 2**: Bob receives the photons and randomly chooses a basis (rectilinear or diagonal) to measure them.
    - **Step 3**: Alice and Bob compare their measurement choices publicly (but not the actual values), keeping only the bits where their bases matched.
3. **Eavesdropping Detection**:
    
    - Any attempt by **Eve** (the eavesdropper) to intercept and measure the quantum states will disturb the system. When Alice and Bob compare their basis choices, discrepancies reveal **Eve's presence**.
4. **Security**:
    
    - BB84’s security is rooted in the **no-cloning theorem** and the fact that any attempt at measurement **disturbs** the quantum state. The act of eavesdropping causes observable errors, ensuring that Alice and Bob can detect the interference.

---

### 🌟 **Six States Protocol**: The Expansion of BB84 🌟

The **Six States Protocol** extends the brilliance of BB84, proposed by Wiesner in 1983, by incorporating **six distinct quantum states** and offering greater flexibility and security. The introduction of **three bases** for measurement rather than just two enhances the robustness of this quantum key distribution scheme.

#### **How Six States Works:**

It builds upon the concept of quantum superposition, where **six states** are used, spread across **three measurement bases**: rectilinear, diagonal, and circular.

1. **Quantum States**:
    
    - The six possible quantum states are:
        - **|0⟩, |1⟩** (rectilinear)
        - **|+⟩, |−⟩** (diagonal)
        - **|R⟩, |L⟩** (right and left circular polarizations)
    
    These states are not just limited to binary encoding; they make use of the **three basis systems**, greatly enhancing security by expanding the number of distinguishable quantum states.
    
2. **Key Generation Process**:
    
    - **Step 1**: Alice prepares the photons randomly from one of the six states.
    - **Step 2**: Bob measures the received photons by randomly selecting one of the three bases.
    - **Step 3**: After transmission, Alice and Bob compare their bases over a public channel, discarding bits when different bases were used.
3. **Eavesdropping Detection**:
    
    - Any eavesdropper would be unable to perfectly measure these non-orthogonal states without disturbing them. The discrepancies between the measurement results indicate whether **Eve** is trying to intercept the communication.
4. **Security**: The **six states** introduce **greater uncertainty** for Eve, making it **even harder** to intercept the quantum transmission without detection. Just like BB84, any disturbance in the quantum state becomes evident during the basis comparison, ensuring **secure key exchange**.
    

---

### ✨ **BB92 Protocol**: Simplicity in Security ✨

BB92, proposed in **1992 by Bennett**, is a more **simplified** and **elegant** version of BB84, relying on just **two non-orthogonal quantum states**. It might seem minimalistic at first, but its elegance and strength lie in its **reduced complexity** while maintaining strong security guarantees.

#### **How BB92 Works:**

BB92 operates with only two states, but its power lies in the fact that these states are **non-orthogonal**, making them **impossible to perfectly distinguish**. This uncertainty protects the transmission from eavesdropping.

1. **Quantum States**:
    
    - BB92 uses just **two non-orthogonal quantum states**:
        - **|0⟩** and **|+⟩**
    
    These states are designed to be **non-orthogonal** so that they cannot be precisely distinguished, providing a strong layer of security.
    
2. **Key Generation Process**:
    
    - **Step 1**: Alice sends photons prepared in one of the two possible states (|0⟩ or |+⟩).
    - **Step 2**: Bob randomly chooses one of two bases (rectilinear or diagonal) to measure the received photons.
    - **Step 3**: Alice and Bob compare their measurement choices publicly and discard bits where their bases do not match.
3. **Eavesdropping Detection**:
    
    - Due to the non-orthogonal nature of the states, any interception by **Eve** will **disturb the state**, causing errors that are **detectable** by Alice and Bob.
4. **Security**: The security of BB92 is derived from the **uncertainty principle** and the non-orthogonality of the states. Any attempt to measure the quantum states by Eve results in **error introduction**, making it impossible for her to gain any useful information without detection.
    

---

### 🔑 **Summary of Key Differences:**

|**Protocol**|**States**|**Bases**|**Key Security Feature**|
|---|---|---|---|
|**BB84**|4 states:|0⟩,|1⟩,|
|**Six States**|6 states:|0⟩,|1⟩,|
|**BB92**|2 non-orthogonal states:|0⟩,|+⟩|
