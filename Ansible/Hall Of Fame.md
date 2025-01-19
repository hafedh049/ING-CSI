# QCM

![[Pasted image 20250118110958.png]]

![[Pasted image 20250118113724.png]]

![[Pasted image 20250118113802.png]]

![[Pasted image 20250118113925.png]]

![[Pasted image 20250118114113.png]]

![[Pasted image 20250118114308.png]]

![[Pasted image 20250118114454.png]]

![[Pasted image 20250118114542.png]]

![[Pasted image 20250118114626.png]]

![[Pasted image 20250118114834.png]]

![[Pasted image 20250118154911.png]]

![[Pasted image 20250118155022.png]]

![[Pasted image 20250118155234.png]]

![[Pasted image 20250118155546.png]]

![[Pasted image 20250118155640.png]]

![[Pasted image 20250118155747.png]]

![[Pasted image 20250118155810.png]]

![[Pasted image 20250118155911.png]]

![[Pasted image 20250118164955.png]]

![[Pasted image 20250118165032.png]]

# Exercice I

![[Pasted image 20250118165225.png]]
### 1. Afficher tous les facts (Display all facts)

```bash
ansible all -m ansible.builtin.setup
```

---

### 2. Vérifier l’installation du paquet `httpd` (Verify the installation of the `httpd` package)

```bash
ansible all -m ansible.builtin.package_facts
```

- This gathers package-related facts, including whether `httpd` is installed.

Alternatively, check specifically for `httpd`:

```bash
ansible all -a "rpm -q httpd" -b
```

---

### 3. Exécuter la commande `rpm -qa | grep http` (Execute the command `rpm -qa | grep http`)

```bash
ansible all -a "rpm -qa | grep http" -b
```

---

### 4. Démarrer et activer le service `httpd` (Start and enable the `httpd` service)

```bash
ansible all -m ansible.builtin.service -a "name=httpd state=started enabled=yes" -b
```

---

### 5. Supprimer l’utilisateur `student` (Remove the user `student`)

```bash
ansible all -m ansible.builtin.user -a "name=student state=absent" -b
```


![[Pasted image 20250119003340.png]]

```yaml
---
- name: Exercise 2 - Manage Groups, Users, and Cron Jobs with Lists
  hosts: all
  become: yes

  vars:
    groups:
      - { name: cpi, gid: 5000 }
      - { name: tic, gid: 5001 }
      - { name: ssir, gid: 5002 }

    users:
      - { name: user1, uid: 1200, group: cpi, shell: /bin/sh }
      - { name: user2, uid: 1300, group: tic, shell: /bin/sh }
      - { name: user3, uid: 1400, group: ssir, shell: /bin/sh }

    cron_jobs:
      - { name: "Write 'hello' to test.txt every 15th of the month, every Friday at 12:14",
          minute: "14", hour: "12", day: "15", weekday: "5", job: "echo hello > /path/to/test.txt" }
      - { name: "Clean /tmp on July 2nd at 9am",
          minute: "0", hour: "9", day: "2", month: "7", job: "rm -rf /tmp/*" }

  tasks:
    - name: Create groups
      ansible.builtin.group:
        name: "{{ item.name }}"
        gid: "{{ item.gid }}"
      loop: "{{ groups }}"

    - name: Create users
      ansible.builtin.user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        group: "{{ item.group }}"
        shell: "{{ item.shell }}"
      loop: "{{ users }}"

    - name: Schedule cron jobs
      ansible.builtin.cron:
        name: "{{ item.name }}"
        minute: "{{ item.minute }}"
        hour: "{{ item.hour }}"
        day: "{{ item.day | default('*') }}"
        month: "{{ item.month | default('*') }}"
        weekday: "{{ item.weekday | default('*') }}"
        job: "{{ item.job }}"
      loop: "{{ cron_jobs }}"
```

