The `nmap` library in Python allows you to run and parse Nmap scans programmatically. Here's a list of the main functions in the Python `nmap` library along with code examples.

---

## 1. **Installation**

Make sure to install the library first:
```bash
pip install python-nmap
```

---

## 2. **Functions in the `nmap.PortScanner` Class**

### 1. **`scan()`**
- This function runs a scan with Nmap.
- **Parameters:** IP address, ports, and Nmap arguments.

**Example:**
```python
import nmap

scanner = nmap.PortScanner()
result = scanner.scan('127.0.0.1', '22-443', '-sV')
print(result)
```

---

### 2. **`all_hosts()`**
- Returns a list of all hosts found in the scan results.

**Example:**
```python
print(scanner.all_hosts())
```

---

### 3. **`has_host()`**
- Checks if a particular host exists in the scan results.

**Example:**
```python
if scanner.has_host('127.0.0.1'):
    print("Host found!")
```

---

### 4. **`hostname()`**
- Returns the hostname of the target IP.

**Example:**
```python
host = '127.0.0.1'
print(f"Hostname: {scanner[host].hostname()}")
```

---

### 5. **`state()`**
- Returns the status of a host (e.g., *up*, *down*).

**Example:**
```python
print(f"Host state: {scanner['127.0.0.1'].state()}")
```

---

### 6. **`all_protocols()`**
- Lists all protocols found in the scan.

**Example:**
```python
print(scanner['127.0.0.1'].all_protocols())
```

---

### 7. **`get_open_ports()`**
- Retrieves open ports for a specific protocol (like TCP/UDP).

**Example:**
```python
open_ports = scanner['127.0.0.1']['tcp'].keys()
print(f"Open TCP ports: {list(open_ports)}")
```

---

### 8. **`version()`**
- Returns the Nmap version used.

**Example:**
```python
print(scanner.nmap_version())
```

---

### 9. **`command_line()`**
- Shows the command used to run the last scan.

**Example:**
```python
print(scanner.command_line())
```

---

### 10. **`scaninfo()`**
- Provides detailed information about the scan (e.g., ports scanned).

**Example:**
```python
print(scanner.scaninfo())
```

---

### 11. **`nmap_version()`**
- Returns the Nmap version tuple used.

**Example:**
```python
print(scanner.nmap_version())
```

---

### 12. **`is_successful()`**
- Checks if the scan was successful.

**Example:**
```python
if scanner.scan('127.0.0.1', '80').get('scan'):
    print("Scan was successful!")
```

---

## Summary of Key Methods

| Method              | Purpose                               |
|---------------------|---------------------------------------|
| `scan()`            | Run Nmap scan.                       |
| `all_hosts()`       | List all discovered hosts.           |
| `has_host()`        | Check if a host exists.              |
| `hostname()`        | Get the hostname of a target.        |
| `state()`           | Get the status of the host (up/down).|
| `all_protocols()`   | List all protocols scanned.          |
| `get_open_ports()`  | Get open ports on the host.          |
| `command_line()`    | View Nmap command used.              |
| `scaninfo()`        | Get scan metadata.                   |
| `nmap_version()`    | Get Nmap version.                    |
