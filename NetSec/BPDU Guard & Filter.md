### **BPDU Guard and BPDU Filter Configuration**

#### **What is BPDU Guard?**

BPDU Guard is a Cisco feature that protects a network from misconfigurations and malicious attacks by disabling a port when it receives a Bridge Protocol Data Unit (BPDU). This is commonly applied on **portfast** ports, which are typically access ports that should not participate in spanning-tree operations.

---

### **What is BPDU Filter?**

BPDU Filter is a feature that prevents the sending and receiving of BPDUs on a port. It can be used to prevent spanning-tree topology changes on specific interfaces, often applied to access ports configured with **portfast**.

---

### **Key Use Cases**

1. **BPDU Guard**:
    
    - Prevents access ports from receiving BPDUs.
    - Protects against accidental or malicious connections of switches to access ports.
2. **BPDU Filter**:
    
    - Stops BPDUs from being sent or received on a port.
    - Typically used in scenarios where BPDUs are unnecessary, such as edge networks.

---

### **Differences Between BPDU Guard and BPDU Filter**

|Feature|BPDU Guard|BPDU Filter|
|---|---|---|
|**Action**|Disables the port on BPDU receipt.|Prevents sending/receiving BPDUs.|
|**Use Case**|Protect access ports from switches.|Disable spanning-tree operations.|

---

### **Configuration Steps**

#### **1. Enable BPDU Guard**

##### **Globally**

BPDU Guard can be enabled globally for all portfast-enabled interfaces:

```c
spanning-tree portfast bpduguard default
```

Example:

```c
conf t
spanning-tree portfast bpduguard default
exit
```

##### **Per-Interface**

BPDU Guard can be enabled on a specific interface:

```c
interface <interface-id>
spanning-tree bpduguard enable
```

Example:

```c
interface f0/1
spanning-tree bpduguard enable
```

---

#### **2. Enable BPDU Filter**

##### **Globally**

BPDU Filter can be enabled globally for all portfast-enabled interfaces:

```c
spanning-tree portfast bpdufilter default
```

Example:

```c
conf t
spanning-tree portfast bpdufilter default
exit
```

##### **Per-Interface**

BPDU Filter can be enabled on a specific interface:

```c
interface <interface-id>
spanning-tree bpdufilter enable
```

Example:

```c
interface f0/1
spanning-tree bpdufilter enable
```

---

### **Verification Commands**

1. **Check BPDU Guard/Filter Status**:
    
    ```c
    show spanning-tree summary
    ```
    
2. **Verify Port Status**:
    
    ```c
    show spanning-tree interface <interface-id> detail
    ```
    
3. **Check for Error-Disabled Ports (if BPDU Guard is triggered)**:
    
    ```c
    show interfaces status | include err-disabled
    ```
    

---

### **Example Configuration**

#### **Scenario 1: BPDU Guard**

Enable BPDU Guard on a portfast-enabled interface to prevent BPDUs:

```c
interface f0/1
spanning-tree portfast
spanning-tree bpduguard enable
```

---

#### **Scenario 2: BPDU Filter**

Enable BPDU Filter globally to prevent sending/receiving BPDUs on portfast interfaces:

```c
conf t
spanning-tree portfast bpdufilter default
```

---

#### **Scenario 3: BPDU Guard and BPDU Filter**

Enable both BPDU Guard and BPDU Filter on a specific interface:

```c
interface f0/1
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
```

---

### **How to Configure in GNS3**

#### **1. Setup the Topology**

1. Add a switch and two PCs.
2. Configure one PC on an access port with portfast enabled.
3. Simulate a rogue switch by connecting another switch to the configured port.

---

#### **2. Configure BPDU Guard**

1. Enable portfast on the access port:
    
    ```c
    interface f0/1
    spanning-tree portfast
    spanning-tree bpduguard enable
    ```
    
2. Test by connecting another switch to this port and observe the port shutdown.

---

#### **3. Configure BPDU Filter**

1. Enable portfast and BPDU filter on the access port:
    
    ```c
    interface f0/1
    spanning-tree portfast
    spanning-tree bpdufilter enable
    ```
    
2. Test by observing the absence of BPDU exchange between connected devices.

---

### **Testing and Troubleshooting**

1. **Trigger BPDU Guard**:
    
    - Connect a switch to an access port configured with BPDU Guard.
    - Verify the port is error-disabled:
        
        ```c
        show interfaces status
        ```
        
2. **Verify BPDU Filter**:
    
    - Ensure no BPDUs are sent or received on the configured port:
        
        ```c
        debug spanning-tree events
        ```
        
3. **Recover from Error-Disabled State (if BPDU Guard is triggered)**:
    
    - Manually re-enable the port:
        
        ```c
        interface f0/1
        shutdown
        no shutdown
        ```
        

---
