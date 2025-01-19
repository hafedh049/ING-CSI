### 1. **`debug`**

- **Purpose**: Prints statements or variable values for debugging purposes.

**Attributes**:

| Attribute   | Description                                       |
| ----------- | ------------------------------------------------- |
| `msg`       | The message to display.                           |
| `var`       | Displays the value of a variable.                 |
| `verbosity` | Limits the message to a specific verbosity level. |

**Example**:

```yaml
- name: Print a debug message
  ansible.builtin.debug:
    msg: "This is a debug message!"

- name: Display a variable's value
  ansible.builtin.debug:
    var: my_variable
```

---

### 2. **`command`**

- **Purpose**: Executes commands on remote nodes without using a shell.

**Attributes**:

|Attribute|Description|
|---|---|
|`cmd`|The command to execute (default).|
|`chdir`|Change to this directory before running.|
|`creates`|Only run if the file does not exist.|
|`removes`|Only run if the file exists.|

**Example**:

```yaml
- name: Run a simple command
  ansible.builtin.command:
    cmd: ls -l

- name: Run a command in a specific directory
  ansible.builtin.command:
    cmd: ls
    chdir: /tmp
```

---

### 3. **`replace`**

- **Purpose**: Replaces text in a file using a regular expression.

**Attributes**:

|Attribute|Description|
|---|---|
|`path`|The file to modify.|
|`regexp`|The regular expression to search for.|
|`replace`|The replacement string.|
|`backup`|Creates a backup of the file before editing.|

**Example**:

```yaml
- name: Replace "foo" with "bar" in a file
  ansible.builtin.replace:
    path: /etc/example.conf
    regexp: "foo"
    replace: "bar"
    backup: yes
```

---

### 4. **`lineinfile`**

- **Purpose**: Ensures a specific line exists in a file or modifies it.

**Attributes**:

|Attribute|Description|
|---|---|
|`path`|The file to modify.|
|`line`|The line to insert or ensure exists.|
|`regexp`|Searches for an existing line to replace.|
|`state`|If `absent`, removes matching lines.|
|`insertafter`|Adds the line after a matching regex or at EOF.|
|`insertbefore`|Adds the line before a matching regex or at BOF.|

**Example**:

```yaml
- name: Add a line to a file
  ansible.builtin.lineinfile:
    path: /etc/example.conf
    line: "new_line_text"
    insertafter: EOF
```

---

### 5. **`ping`**

- **Purpose**: Tests the connectivity between the control node and managed nodes.

**Attributes**: No specific attributes.

**Example**:

```yaml
- name: Ping all managed nodes
  ansible.builtin.ping:
```

---

### 6. **`setup`**

- **Purpose**: Gathers facts about the system (e.g., OS, IP, CPU).

**Attributes**:

|Attribute|Description|
|---|---|
|`filter`|Collects specific facts.|
|`gather_subset`|Limits facts to a specific subset (e.g., `min`).|
|`gather_timeout`|Time in seconds to wait for fact gathering.|

**Example**:

```yaml
- name: Gather facts about a system
  ansible.builtin.setup:
    filter: "ansible_distribution"
```

---

### 7. **`dnf` / `yum` / `apt` / `package`**

- **Purpose**: Manages software packages.

**Attributes**:

|Attribute|Description|
|---|---|
|`name`|The package to manage (can be a list).|
|`state`|Desired state: `present`, `absent`, or `latest`.|
|`allow_downgrade`|Whether downgrades are allowed.|

**Example (DNF)**:

```yaml
- name: Install a package
  ansible.builtin.dnf:
    name: httpd
    state: present
```

**Example (APT)**:

```yaml
- name: Remove a package
  ansible.builtin.apt:
    name: nginx
    state: absent
```

---

### 8. **`user`**

- **Purpose**: Manages user accounts on the system.

**Attributes**:

|Attribute|Description|
|---|---|
|`name`|The username.|
|`state`|Desired state: `present` or `absent`.|
|`uid`|The user's ID.|
|`group`|The primary group.|
|`password`|The hashed password.|

**Example**:

```yaml
- name: Create a user
  ansible.builtin.user:
    name: john
    state: present
    password: "{{ 'password' | password_hash('sha512') }}"
```

---

### 9. **`group`**

- **Purpose**: Manages groups on the system.

**Attributes**:

|Attribute|Description|
|---|---|
|`name`|The group name.|
|`state`|Desired state: `present` or `absent`.|
|`gid`|The group's ID.|

**Example**:

```yaml
- name: Create a group
  ansible.builtin.group:
    name: developers
    state: present
```

---

### 10. **`service`**

- **Purpose**: Manages services on the system.

**Attributes**:

|Attribute|Description|
|---|---|
|`name`|The name of the service.|
|`state`|Desired state: `started`, `stopped`, or `restarted`.|
|`enabled`|Whether the service starts at boot.|

**Example**:

```yaml
- name: Start and enable Apache
  ansible.builtin.service:
    name: httpd
    state: started
    enabled: yes
```

---

### 11. **`firewalld`**

- **Purpose**: Manages firewall rules.

**Attributes**:

|Attribute|Description|
|---|---|
|`service`|The service to allow/deny.|
|`state`|Desired state: `enabled` or `disabled`.|
|`zone`|The firewall zone to apply the rule.|

**Example**:

```yaml
- name: Allow HTTP service through the firewall
  ansible.builtin.firewalld:
    service: http
    state: enabled
    permanent: yes
```

---

### 12. **`copy`**

- **Purpose**: Copies files from the control node to managed nodes.

**Attributes**:

|Attribute|Description|
|---|---|
|`src`|Source file path.|
|`dest`|Destination file path.|
|`owner`|Owner of the file.|
|`group`|Group of the file.|
|`mode`|Permissions for the file.|

**Example**:

```yaml
- name: Copy a configuration file
  ansible.builtin.copy:
    src: /path/to/source.conf
    dest: /etc/app/config.conf
    owner: root
    group: root
    mode: '0644'
```

---
### 13. **`cron`**

- **Purpose**: Manages cron jobs for scheduling tasks.

**Attributes**:

|Attribute|Description|
|---|---|
|`name`|A unique name for the cron job.|
|`minute`|Minute when the job runs (e.g., `0`, `*/5`).|
|`hour`|Hour when the job runs (e.g., `2`, `*`).|
|`day`|Day of the month when the job runs (e.g., `1`, `*`).|
|`month`|Month when the job runs (e.g., `1`, `*`).|
|`weekday`|Day of the week when the job runs (e.g., `0` for Sunday).|
|`job`|The command or script to execute.|
|`state`|`present` to add or update the job, `absent` to remove it.|
|`user`|The user to manage the cron job for (default is `root`).|

**Example**:

```yaml
- name: Create a cron job to run a backup script every day at 2 AM
  ansible.builtin.cron:
    name: "Daily backup"
    minute: "0"
    hour: "2"
    job: "/usr/local/bin/backup.sh"
    state: present

- name: Remove a specific cron job
  ansible.builtin.cron:
    name: "Daily backup"
    state: absent
```

---

### 14. **`uri`**

- **Purpose**: Interacts with web services or APIs.

**Attributes**:

|Attribute|Description|
|---|---|
|`url`|The URL to interact with.|
|`method`|The HTTP method to use (e.g., `GET`, `POST`, `PUT`, `DELETE`).|
|`headers`|HTTP headers to include in the request (as a dictionary).|
|`body`|The body content for the request (for `POST`, `PUT`, etc.).|
|`body_format`|Specifies the body format (`raw`, `json`, `form-urlencoded`).|
|`status_code`|Expected HTTP status codes (e.g., `[200, 201]`).|
|`return_content`|If `yes`, returns the body of the response.|
|`timeout`|Time (in seconds) before the request times out.|
|`validate_certs`|If `no`, ignores SSL certificate validation (useful for self-signed certificates).|

**Example**:

```yaml
- name: Perform a GET request to fetch data from an API
  ansible.builtin.uri:
    url: "https://api.example.com/data"
    method: GET
    return_content: yes

- name: Send a POST request with JSON data
  ansible.builtin.uri:
    url: "https://api.example.com/create"
    method: POST
    headers:
      Content-Type: "application/json"
    body: '{"name": "John", "age": 30}'
    body_format: json
    status_code: [200, 201]
    validate_certs: yes
```
