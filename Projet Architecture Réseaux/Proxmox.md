### 🚀 **Steps to Create a VM in Proxmox Without ISO**

#### 🛠️ 1. **Create a New Empty VM**

- Go to **Proxmox Web UI**.
    
- Click **Create VM**.
    
- In the wizard:
    
    - **Node:** Pick your Proxmox node.
        
    - **VM ID & Name:** Choose a unique ID and name.
        
    - **OS tab:** Select **"Do not use any media"** or leave ISO blank.
        
    - **System:** Leave defaults or match the source VM settings.
        
    - **Hard Disk:** You can leave it empty or minimal — you’ll replace it anyway.
        
    - **CPU, Memory, Network:** Set according to the original VM.
        
    - **Confirm/Create** the VM.
        

#### 📦 2. **Extract Your `.ova` File**

SSH into your Proxmox host and extract the `.ova`:

```bash
tar -xvf your-vm.ova
```

You’ll get files like:

- `your-vm.ovf`
    
- `your-vm-disk1.vmdk`
    

#### 🔁 3. **Import the VMDK Disk into the VM**

Use this command to attach the disk:

```bash
qm importdisk <VMID> your-vm-disk1.vmdk local-lvm
```

Example:

```bash
qm importdisk 101 your-vm-disk1.vmdk local-lvm
```

💡 Replace `local-lvm` with your actual Proxmox storage (check under **Datacenter > Storage**).

#### ⚙️ 4. **Attach the Imported Disk in Web UI**

- Go to the new VM > **Hardware**.
    
- Click **Add > Hard Disk**.
    
- Choose the imported disk.
    
- Set **bus type** to `SATA`, `SCSI`, or `VirtIO` (match original VM setup).
    
- Save.
    

#### 🔄 5. **Set Boot Order**

- Go to **Options > Boot Order**.
    
- Set the imported disk as first.
    

#### ▶️ 6. **Start the VM**

- Hit **Start** and watch it boot 🎉
    

---

