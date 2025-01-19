For a simple Ansible inventory with just groups and hostnames, here's the basic structure and syntax for your `ansible.cfg` with only `defaults` and `privilege_escalation` sections.

### Inventory file (e.g., `inventory.ini`):

```c
[group1]
hostname1
hostname2

[group2]
hostname3
hostname4
```

### `ansible.cfg` configuration:

```c
[defaults]
inventory = /path/to/inventory.ini
host_key_checking = False
```

In this example:

- `inventory` points to the path of your inventory file.
- `host_key_checking` is disabled to avoid prompts for SSH key verification.

If you need to handle privilege escalation (e.g., using `sudo`), you can add this to the `defaults` section in `ansible.cfg`:

```css
[privilege_escalation]
become = True | true | yes
become_method = sudo
become_user = root
```

This configuration will allow Ansible to use `sudo` for privilege escalation on remote hosts.
