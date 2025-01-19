### **Using Built-in Facts in Ansible**

Ansible collects facts from managed nodes using the `setup` module. These facts can be accessed in playbooks or templates using `{{ }}` syntax.

#### **Example of Using Built-in Facts**

```css
- name: Use Ansible facts
  hosts: all
  tasks:
    - name: Print OS distribution
      ansible.builtin.debug:
        msg: "This machine is running {{ ansible_distribution }} {{ ansible_distribution_version }}"

    - name: Check total memory
      ansible.builtin.debug:
        msg: "Total memory available: {{ ansible_memtotal_mb }} MB"
```

### **Creating Custom Facts**

Custom facts allow you to define additional information about your managed nodes.

---

#### **Method 1: Creating Custom Facts on the Managed Node**

1. Create a directory `/etc/ansible/facts.d` on the managed node.
    
    ```bash
    sudo mkdir -p /etc/ansible/facts.d
    ```
    
2. Add a custom fact file in this directory. The file must have a `.fact` extension and be in JSON, INI, or executable script format.
    

**Example: JSON Custom Fact**

```bash
sudo nano /etc/ansible/facts.d/custom.json
```

```json
{
  "environment": "production",
  "app_version": "1.2.3",
  "owner": "Chikh Jbal"
}
```

**Example: INI Custom Fact**

```bash
sudo nano /etc/ansible/facts.d/custom.fact
```

```ini
[general]
environment=production
app_version=1.2.3
owner=Chikh Jbal
```

3. Ensure the file is readable by Ansible:
    
    ```bash
    sudo chmod 0644 /etc/ansible/facts.d/custom.*
    ```
    
4. Access the custom facts in playbooks:
    

```yaml
- name: Use custom facts
  hosts: all
  tasks:
    - name: Display custom facts
      ansible.builtin.debug:
        msg: "Environment: {{ ansible_local.custom.environment }}, Version: {{ ansible_local.custom.app_version }}"
```

---

#### **Method 2: Creating Custom Facts via Playbooks**

You can also create custom facts dynamically using playbooks.

**Example: Setting Custom Facts**

```css
- name: Create custom facts dynamically
  hosts: all
  become: yes
  tasks:
    - name: Create facts directory
      ansible.builtin.file:
        path: /etc/ansible/facts.d
        state: directory
        mode: '0755'

    - name: Add a custom fact
      ansible.builtin.copy:
        dest: /etc/ansible/facts.d/custom.fact
        content: |
          [general]
          environment=staging
          app_version=2.0.0
          owner=Chikh Jbal
        mode: '0644'

    - name: Display custom fact
      ansible.builtin.debug:
        msg: "Custom environment: {{ ansible_local.custom.environment }}"
```

---

### **Best Practices for Custom Facts**

1. **Naming Conventions**: Use unique names for your custom facts to avoid conflicts with Ansible built-in facts.
2. **Readability**: Use JSON format for complex facts as it supports nesting.
3. **Security**: Restrict permissions on custom fact files to avoid unauthorized access.
4. **Consistency**: Ensure the `/etc/ansible/facts.d` directory exists on all managed nodes for uniform behavior.

---

The correct syntax for using **facts** in Ansible with both `.` and `[]` styles depends on how you access the variables. Here’s an explanation and examples:

---

### **1. Dot (`.`) Syntax**

- **Purpose**: Commonly used when the fact key is alphanumeric and does not contain special characters.
- **Example**:
    
    ```yaml
    ansible_distribution  # Accesses the OS distribution
    ansible_default_ipv4.address  # Accesses the primary IP address
    ```
    
- **When to Use**:
    - Fact keys are simple and alphanumeric.

#### **Example with Playbook**:

```yaml
- name: Use dot syntax
  hosts: all
  tasks:
    - name: Print the OS distribution
      ansible.builtin.debug:
        msg: "OS: {{ ansible_distribution }}"

    - name: Print the default IPv4 address
      ansible.builtin.debug:
        msg: "IP Address: {{ ansible_default_ipv4.address }}"
```

---

### **2. Bracket (`[]`) Syntax**

- **Purpose**: Used when the fact key contains special characters, spaces, or is dynamically generated.
- **Example**:
    
    ```yaml
    ansible_facts['ansible_distribution']  # Equivalent to ansible_distribution
    ansible_facts['ansible_default_ipv4']['address']  # Equivalent to ansible_default_ipv4.address
    ```
    
- **When to Use**:
    - Fact keys include spaces, special characters, or start with a number.
    - You need to dynamically generate fact keys.

#### **Example with Playbook**:

```yaml
- name: Use bracket syntax
  hosts: all
  tasks:
    - name: Print the OS distribution
      ansible.builtin.debug:
        msg: "OS: {{ ansible_facts['ansible_distribution'] }}"

    - name: Print the default IPv4 address
      ansible.builtin.debug:
        msg: "IP Address: {{ ansible_facts['ansible_default_ipv4']['address'] }}"
```

---

### **3. Dynamic Access with Variables**

You can use a variable inside brackets for dynamic fact access.

#### **Example**:

```yaml
- name: Use dynamic fact access
  hosts: all
  vars:
    fact_name: "ansible_distribution"
  tasks:
    - name: Print dynamically accessed fact
      ansible.builtin.debug:
        msg: "Dynamic Fact: {{ ansible_facts[fact_name] }}"
```

---

### **4. Mixing Syntax**

Dot and bracket syntax can be mixed if necessary, though it’s not common.

#### **Example**:

```yaml
- name: Mix dot and bracket syntax
  hosts: all
  tasks:
    - name: Access nested facts
      ansible.builtin.debug:
        msg: "IP Address: {{ ansible_facts['ansible_default_ipv4'].address }}"
```

---

### **Which Syntax Should I Use?**

- **Prefer Dot Syntax**: For simplicity and readability when keys are alphanumeric.
- **Use Bracket Syntax**: For special cases like:
    - Keys with special characters or spaces.
    - Dynamically generated keys.

Both styles can coexist in a playbook but use them appropriately based on the structure of the fact or variable.
