### 🔧 What You’ll Need:

- **Python** (obviously 😅)
    
- A file like `users.csv` or `users.json` with the user data
    
- A library like `ldap3` (most common for talking to AD)
    

---

### 📦 Example with `ldap3` and a CSV file:

```python
import csv
from ldap3 import Server, Connection, ALL, NTLM

# 🔐 Connection setup
server = Server('your.ad.domain', get_info=ALL)
conn = Connection(
    server,
    user='DOMAIN\\admin_user',
    password='your_password',
    authentication=NTLM,
    auto_bind=True
)

# 📂 Load users from CSV
with open('users.csv', newline='') as csvfile:
    reader = csv.DictReader(csvfile)
    for row in reader:
        dn = f"CN={row['username']},OU=Users,DC=your,DC=domain"
        attrs = {
            'objectClass': ['top', 'person', 'organizationalPerson', 'user'],
            'cn': row['username'],
            'givenName': row['first_name'],
            'sn': row['last_name'],
            'displayName': f"{row['first_name']} {row['last_name']}",
            'userPrincipalName': f"{row['username']}@your.domain",
            'sAMAccountName': row['username'],
            'mail': row['email'],
        }
        conn.add(dn, attributes=attrs)

        # 👇 Set password (requires extra rights)
        conn.extend.microsoft.modify_password(dn, row['password'])

        # 🔓 Enable account (AD disables accounts by default)
        conn.modify(dn, {'userAccountControl': [(2, 512)]})  # 512 = NORMAL_ACCOUNT

print("🔥 Done creating users!")
```

---

### 🔐 CSV Format Example (`users.csv`):

```
username,first_name,last_name,email,password
jdoe,John,Doe,jdoe@example.com,SuperSecret123!
asmith,Alice,Smith,asmith@example.com,Passw0rd!
```
