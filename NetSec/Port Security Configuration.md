### **Port Security Configuration on Cisco Switch**

#### **What is Port Security?**
Port Security is a Layer 2 security feature that restricts access to a switch port based on the MAC addresses of devices connected to it. This prevents unauthorized devices from accessing the network and can help mitigate MAC address spoofing or rogue device threats.

---

### **Key Features of Port Security**
1. **MAC Address Limiting**: Restrict the number of MAC addresses allowed on a port.
2. **Action on Violation**:
   - **Protect**: Drops packets from unknown MAC addresses but doesn’t log the violation.
   - **Restrict**: Drops packets from unknown MAC addresses and logs the violation.
   - **Shutdown**: Puts the port into an error-disabled state (default action).

3. **MAC Address Types**:
   - **Static**: Manually configured MAC addresses.
   - **Dynamic**: Automatically learned MAC addresses cleared when the switch restarts.
   - **Sticky**: Dynamically learned MAC addresses that persist after a restart.

---

### **Steps to Configure Port Security**

#### **Step 1: Enter Interface Configuration Mode**
Select the interface where you want to enable port security:
```c
interface <interface-id>
```

Example:
```c
interface f0/1
```

---

#### **Step 2: Enable Port Security**
Activate port security on the interface:
```c
switchport port-security
```

---

#### **Step 3: Set Port Security Options**

1. **Specify the Maximum Number of MAC Addresses**:
   ```c
   switchport port-security maximum <number>
   ```

   Example:
   ```c
   switchport port-security maximum 2
   ```

2. **Configure Violation Mode**:
   ```c
   switchport port-security violation {protect | restrict | shutdown}
   ```

   Example:
   ```c
   switchport port-security violation shutdown
   ```

3. **Add MAC Addresses**:
   - **Static MAC Address**:
     ```c
     switchport port-security mac-address <mac-address>
     ```
     Example:
     ```c
     switchport port-security mac-address 0001.2345.6789
     ```
   - **Sticky MAC Address**:
     Enable sticky learning for dynamic MAC addresses:
     ```c
     switchport port-security mac-address sticky
     ```

---

#### **Step 4: Save Configuration**
Save the running configuration to ensure the settings persist:
```c
write memory
```

---

### **Verification Commands**
1. **Verify Port Security Settings**:
   ```c
   show port-security
   ```
2. **Verify Port Security on a Specific Interface**:
   ```c
   show port-security interface <interface-id>
   ```
3. **View Security Violations**:
   ```c
   show port-security address
   ```

---

### **Example Configuration**

#### **Scenario**
- Allow a maximum of 2 MAC addresses.
- Violation mode: Shutdown.
- Enable sticky MAC address learning.

#### **Configuration**
```c
interface f0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address sticky
```

---

### **How to Configure Port Security in GNS3**

#### **1. Setup the Topology**
1. Add a Cisco switch and multiple end devices (e.g., PCs).
2. Connect the PCs to the switch interfaces.

#### **2. Configure Port Security on the Switch**
1. Select the interface connected to the PC:
   ```c
   interface f0/1
   switchport mode access
   switchport port-security
   switchport port-security maximum 2
   switchport port-security violation shutdown
   switchport port-security mac-address sticky
   ```

2. Repeat for other interfaces if required.

---

#### **3. Test the Configuration**
1. **Connect Authorized Devices**: Connect up to 2 authorized devices and check connectivity.
2. **Connect an Unauthorized Device**:
   - Connect a third device or spoof a different MAC address.
   - Observe the violation mode action (e.g., shutdown).

3. **Verify Port Security**:
   - View active MAC addresses:
     ```c
     show port-security
     ```
   - View sticky MAC addresses:
     ```c
     show running-config | include sticky
     ```
   - Check interface status:
     ```c
     show interfaces status
     ```

---

### **Troubleshooting**
1. If a port is in an error-disabled state:
   - Manually re-enable the port:
     ```c
     interface f0/1
     shutdown
     no shutdown
     ```
2. Investigate violations:
   ```c
   show port-security address
   ```

---
