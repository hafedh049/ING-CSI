Here’s the full structure of an Ansible playbook showcasing the specified components:

```yaml
---
- name: Example Playbook for Structured Ansible Tasks
  hosts: webservers
  become: true
  gather_facts: true

  vars:
    package_name: httpd
    service_name: httpd

  vars_files:
    - vars/main.yml

  tasks:
    - name: Install the required package
      ansible.builtin.yum:
        name: "{{ package_name }}"
        state: present

    - name: Start and enable the service
      ansible.builtin.systemd:
        name: "{{ service_name }}"
        state: started
        enabled: true
      notify:
        - Restart {{ service_name }}

    - name: Create a custom index.html file
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: "Hello from Ansible!"
        owner: root
        group: root
        mode: 0644

  handlers:
    - name: Restart {{ service_name }}
      ansible.builtin.systemd:
        name: "{{ service_name }}"
        state: restarted
```

### Explanation of Components:

1. **`name:`**
    
    - Provides a human-readable name for the playbook and tasks.
2. **`hosts:`**
    
    - Defines the target group (e.g., `webservers`) for running the playbook.
3. **`become:`**
    
    - Specifies that tasks will be executed with escalated privileges (e.g., `sudo`).
4. **`gather_facts:`**
    
    - Collects system information about the target machines.
5. **`vars:`**
    
    - Inline variables for the playbook.
    - Example: `package_name` and `service_name`.
6. **`vars_files:`**
    
    - External YAML files that include additional variables.
    - Example: `vars/main.yml`.
7. **`tasks:`**
    
    - The main actions to be performed, such as installing a package, starting a service, or copying files.
8. **`handlers:`**
    
    - Used to define operations that are triggered by the `notify` directive in tasks.
    - Example: Restarting a service after a change.

### Sample `vars/main.yml` File:

```css
---
package_name: nginx
service_name: nginx
```

This structure covers all the required components and demonstrates how they interact within the playbook.