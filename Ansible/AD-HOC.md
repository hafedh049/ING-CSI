### **Ad-Hoc Command Syntax in Ansible**

The general syntax for running an ad-hoc command is:

```bash
ansible <host-pattern> -m <module-name> -a "<module-arguments>" [options]
```

- **`<host-pattern>`**: Specifies the group of hosts or individual host.
- **`<module-name>`**: The name of the Ansible module to run.
- **`<module-arguments>`**: Key-value arguments specific to the module.
- **`[options]`**: Additional options, such as inventory file or verbosity.

---

### **Ad-Hoc Command Examples**

#### 1. **`debug`**

```bash
ansible all -m debug -a "msg='This is a debug message!'"
```

- This command displays a debug message on all hosts.

---

#### 2. **`command`**

```bash
ansible all -m command -a "cmd=uptime"
```

- Runs the `uptime` command on all hosts.

---

#### 3. **`replace`**

```bash
ansible webservers -m replace -a "path=/etc/example.conf regexp='foo' replace='bar' backup=yes"
```

- Replaces the text `foo` with `bar` in `/etc/example.conf` on `webservers`.

---

#### 4. **`lineinfile`**

```bash
ansible all -m lineinfile -a "path=/etc/example.conf line='New configuration line' insertafter=EOF"
```

- Ensures a specific line exists in the file `/etc/example.conf`.

---

#### 5. **`ping`**

```bash
ansible all -m ping
```

- Tests connectivity to all managed nodes.

---

#### 6. **`setup`**

```bash
ansible all -m setup -a "filter=ansible_distribution"
```

- Gathers and displays facts about the OS distribution for all hosts.

---

#### 7. **`dnf` / `yum` / `apt / package`**

**Install a Package with `dnf`:**

```bash
ansible all -m dnf -a "name=httpd state=present"
```

**Remove a Package with `apt`:**

```bash
ansible all -m apt -a "name=nginx state=absent"
```

---

#### 8. **`user`**

```bash
ansible all -m user -a "name=john state=present"
```

- Creates a user named `john` on all hosts.

---

#### 9. **`group`**

```bash
ansible all -m group -a "name=developers state=present"
```

- Creates a group named `developers`.

---

#### 10. **`service`**

```bash
ansible all -m service -a "name=httpd state=started enabled=yes"
```

- Starts and enables the `httpd` service.

---

#### 11. **`ansible.posix.firewalld`**

```bash
ansible all -m firewalld -a "service=http state=enabled permanent=yes"
```

- Enables the HTTP service in the firewall.

---

#### 12. **`copy`**

```bash
ansible all -m copy -a "src=/local/path/file.conf dest=/etc/file.conf mode=0644 owner=root group=root"
```

- Copies a file to all hosts.

---

#### 13. **`cron`**

```bash
ansible all -m cron -a "name='Daily backup' minute=0 hour=2 job='/usr/local/bin/backup.sh'"
```

- Schedules a daily backup job at 2 AM.

---

#### 14. **`uri`**

**GET Request:**

```bash
ansible all -m uri -a "url=https://api.example.com/data"
```
