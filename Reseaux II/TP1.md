# Question I

### **Configuring a Router as an NTP Client in Cisco Packet Tracer**

### **📌 Step 1: Configure the NTP Server**

If you are using a **router as the NTP server**, follow these steps:

```bash
enable
configure terminal
clock set 12:00:00 Feb 16 2025   # Set the correct time
ntp master 1                      # Set the router as NTP Server (Stratum 1)
exit
write memory                      # Save configuration
```

---

### **📌 Step 2: Configure the Router as an NTP Client**

On the **client router**, configure it to sync time with the NTP server:

```bash
enable
configure terminal
ntp server <NTP_SERVER_IP>        # Replace with the NTP server's IP
exit
write memory                      # Save configuration
```

Example: If the NTP server's IP is `192.168.1.1`, use:

```bash
ntp server 192.168.1.1
```

---

### **📌 Step 4: Verify NTP Synchronization**

To check if the router is synchronized with the NTP server, use:

```bash
show ntp status
show clock
show ntp associations
```

--- 

### **Updating the Hardware Clock on a Cisco Router Using `update-calendar` and `do` Commands** 🕒

In Cisco IOS, you can update the **hardware clock (RTC - Real-Time Clock)** using `update-calendar`. Here’s how:

---

### **1️⃣ Check the Current Time on the Router**

Before making any changes, check the system and hardware clock:

```bash
show clock       # Check system time
show calendar    # Check hardware clock (RTC)
```

---

### **2️⃣ Set the System Clock (If Needed)**

If the **system time** is incorrect, update it manually:

```bash
clock set HH:MM:SS DAY MONTH YEAR
```

Example:

```bash
clock set 12:30:00 16 FEB 2025
```

---

### **3️⃣ Update the Hardware Clock (RTC) Using `update-calendar`**

Once the system time is correct, sync it to the hardware clock:

```bash
update-calendar
```

✅ This copies the **system clock** to the **hardware clock (RTC)**.

---

### **4️⃣ Sync Time Using NTP (Optional - Recommended)**

Instead of setting time manually, you can sync with an **NTP server**:

```bash
ntp server <NTP_SERVER_IP>
```

Example:

```bash
ntp server 192.168.1.1
```

Then, update the hardware clock:

```bash
update-calendar
```

---

### **5️⃣ Verify the Update**

After updating, check if the time is correct:

```bash
show clock
show calendar
```

---

### **Using `do` Command in Configuration Mode**

If you're inside **global configuration mode (`config terminal`)** and want to run EXEC commands like `show clock` or `update-calendar`, use **`do`**:

```bash
do show clock
do update-calendar
do show calendar
```

✅ This allows you to run privileged mode commands without exiting config mode.

---

### **💡 Summary**

🔹 `clock set <time>` → Update system clock  
🔹 `update-calendar` → Sync system time to hardware clock  
🔹 `ntp server <IP>` → Sync time with an NTP server  
🔹 `do <command>` → Run EXEC mode commands inside config mode

----

