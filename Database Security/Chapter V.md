# 🎩 **Oracle User & Privilege Management – The Ultimate Guide** 🏛️

## 🔹 **1. Creating a New User** 👤

Before a user can interact with the database, we must **create** them and set a password.

```sql
CREATE USER chikh IDENTIFIED BY "SuperSecure@123";
```

✨ **Optional Enhancements**:

```sql
CREATE USER chikh IDENTIFIED BY "SuperSecure@123"
DEFAULT TABLESPACE users  -- Assign default storage
TEMPORARY TABLESPACE temp  -- Assign temp space
QUOTA unlimited ON users;  -- Allow unlimited space usage
```

🛠️ **Best Practice:** After creating a user, grant necessary privileges so they can perform actions.

---

## 🛡️ **2. Granting Privileges** – Give Users the Power! ⚡

Oracle provides two types of privileges:  
🔹 **System Privileges** – Allow users to perform database-wide actions.  
🔹 **Object Privileges** – Allow users to perform actions on specific tables, views, etc.

### 🔹 **Granting System Privileges**

```sql
GRANT CREATE SESSION TO chikh; -- Allows user to log in
GRANT CREATE TABLE TO chikh;   -- Allows user to create tables
GRANT CREATE VIEW TO chikh;    -- Allows user to create views
GRANT CREATE PROCEDURE TO chikh; -- Allows creating stored procedures
```

💡 **Tip:** Without `CREATE SESSION`, the user **cannot log in** to the database.

### 🔹 **Granting Object Privileges**

If you want a user to **access specific tables**, use:

```sql
GRANT SELECT, INSERT, UPDATE ON employees TO chikh;
```

🔸 **Common Object Privileges**:

- `SELECT` – Read data
- `INSERT` – Add data
- `UPDATE` – Modify data
- `DELETE` – Remove data
- `EXECUTE` – Run stored procedures

👑 **Granting ALL Privileges on a Table**:

```sql
GRANT ALL ON employees TO chikh;
```

🚀 **Supercharged Access:**

```sql
GRANT DBA TO chikh; -- Grants full administrative access (Use with caution!)
```

---

## 🔥 **3. Revoking Privileges** – Taking Back the Power! ⚠️

If a user **no longer needs** a privilege, we can **revoke** it.

### **Revoking System Privileges**

```sql
REVOKE CREATE TABLE FROM chikh;
```

🛑 **User can no longer create tables!**

### **Revoking Object Privileges**

```sql
REVOKE SELECT, INSERT ON employees FROM chikh;
```

🎭 **Revoke Everything** (on a table):

```sql
REVOKE ALL ON employees FROM chikh;
```

---

## 🎭 **4. Creating and Managing Roles** – Because Teamwork Rocks! 🎸

Instead of assigning privileges **individually**, we can group them into **roles**. Roles can be reflexive (Assigned to other roles)

### **Creating a Role**

```sql
CREATE ROLE manager_role;
```

### **Assigning Privileges to the Role**

```sql
GRANT SELECT, INSERT, UPDATE ON employees TO manager_role;
```

### **Granting the Role to Users**

```sql
GRANT manager_role TO chikh;
```

### **Revoking a Role**

```sql
REVOKE manager_role FROM chikh;
```

---

## 🎯 **Final Touch: Deleting a User** 🗑️

```sql
DROP USER chikh CASCADE;
```

💣 **Warning:** The `CASCADE` option **removes all schema objects** owned by the user.

---

## 🎉 **Conclusion**

🔹 `GRANT` gives users **power**  
🔹 `REVOKE` **removes** access  
🔹 `CREATE ROLE` helps **organize** privileges  
🔹 `DROP USER` **eliminates** users

---

Oracle provides several **data dictionary views** related to **users, roles, and privileges**. Below is a categorized list of the most important views:

---

# 📜 **Views Related to Users, Roles, and Privileges in Oracle** 🏛️

## 👤 **1. User Information Views**

These views provide information about **users in the database**.

|View Name|Description|
|---|---|
|`DBA_USERS`|Lists all database users with details like account status, default tablespace, profile, and authentication type.|
|`ALL_USERS`|Lists all users in the database but with fewer details than `DBA_USERS`.|
|`USER_USERS`|Displays information about the **current** user only.|

🔹 **Example Query:**

```sql
SELECT username, account_status, created FROM DBA_USERS;
```

---

## 🔐 **2. Privileges & Permissions Views**

These views show which privileges are assigned to users and roles.

|View Name|Description|
|---|---|
|`DBA_SYS_PRIVS`|Lists all **system privileges** granted to users and roles.|
|`DBA_TAB_PRIVS`|Shows **object-level privileges** (SELECT, INSERT, UPDATE, DELETE) on tables, views, procedures, etc.|
|`USER_SYS_PRIVS`|Displays system privileges granted **to the current user**.|
|`USER_TAB_PRIVS`|Displays table-level privileges granted **to the current user**.|
|`USER_TAB_PRIVS_MADE`|Shows table privileges granted **by the current user** to others.|
|`USER_TAB_PRIVS_RECD`|Shows table privileges received **by the current user**.|

🔹 **Example Query: Who Has System Privileges?**

```sql
SELECT grantee, privilege FROM DBA_SYS_PRIVS WHERE grantee = 'CHIKH';
```

🔹 **Example Query: Who Has Access to a Specific Table?**

```sql
SELECT grantee, privilege FROM DBA_TAB_PRIVS WHERE table_name = 'EMPLOYEES';
```

---

## 🎭 **3. Role Management Views**

These views provide details on **roles assigned to users** and their privileges.

|View Name|Description|
|---|---|
|`DBA_ROLES`|Lists all **roles** available in the database.|
|`DBA_ROLE_PRIVS`|Shows which **roles are assigned** to users or other roles.|
|`USER_ROLE_PRIVS`|Displays the **roles assigned to the current user**.|
|`DBA_SYS_PRIVS`|Lists **system privileges assigned to roles**.|
|`DBA_TAB_PRIVS`|Lists **table privileges assigned to roles**.|

🔹 **Example Query: List All Roles in the Database**

```sql
SELECT role FROM DBA_ROLES;
```

🔹 **Example Query: Find Roles Assigned to a User**

```sql
SELECT grantee, granted_role FROM DBA_ROLE_PRIVS WHERE grantee = 'CHIKH';
```

---

## 📊 **4. Views for Auditing & Security**

These views help in monitoring **who has access to what**.

|View Name|Description|
|---|---|
|`DBA_AUDIT_TRAIL`|Shows **audit logs** (if auditing is enabled).|
|`DBA_PROFILES`|Lists **profile settings** for password policies and session limits.|
|`DBA_USERS_WITH_DEFPWD`|Identifies users **with default passwords** (potential security risk).|

🔹 **Example Query: Show Users with Default Passwords**

```sql
SELECT username FROM DBA_USERS_WITH_DEFPWD;
```

---

# 🎯 **Summary**

✅ **User Management Views:** `DBA_USERS`, `ALL_USERS`, `USER_USERS`  
✅ **Privilege Views:** `DBA_SYS_PRIVS`, `USER_SYS_PRIVS`, `DBA_TAB_PRIVS`, `USER_TAB_PRIVS`  
✅ **Role Management Views:** `DBA_ROLES`, `DBA_ROLE_PRIVS`, `USER_ROLE_PRIVS`  
✅ **Auditing & Security Views:** `DBA_AUDIT_TRAIL`, `DBA_PROFILES`, `DBA_USERS_WITH_DEFPWD`

These views help **monitor, manage, and secure** users, roles, and privileges effectively. 🚀