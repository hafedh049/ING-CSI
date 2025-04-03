In **Oracle SQL*Plus**, **tablespaces** are used to manage the logical storage of database objects. Below is a detailed list of commands related to **tablespaces**, including **creation, modification, management, and deletion**.

---

### **1. Creating a Tablespace**

#### **Permanent Tablespace**

```sql
CREATE TABLESPACE my_tablespace 
DATAFILE '/u01/oradata/my_tablespace.dbf' 
SIZE 100M 
AUTOEXTEND ON 
NEXT 50M 
MAXSIZE 500M;
```

- `DATAFILE`: Specifies the file location.
- `SIZE`: Initial size of the tablespace.
- `AUTOEXTEND ON`: Allows the file to grow automatically.
- `NEXT`: Specifies the increment for auto-extension.
- `MAXSIZE`: Sets the maximum allowed size.
#### **Temporary Tablespace**

```sql
CREATE TEMPORARY TABLESPACE temp_space 
TEMPFILE '/u01/oradata/temp_space.dbf' 
SIZE 200M 
AUTOEXTEND ON 
NEXT 100M 
MAXSIZE UNLIMITED;
```

- `TEMPORARY`: Defines a temporary tablespace for sorting operations.
- `TEMPFILE`: Specifies the temp file.

#### **Undo Tablespace**

```sql
CREATE UNDO TABLESPACE undo_space 
DATAFILE '/u01/oradata/undo_space.dbf' 
SIZE 500M 
AUTOEXTEND ON 
NEXT 100M 
MAXSIZE 2G;
```

- Used for **rollback and undo management**.

---

### **2. Altering a Tablespace**

#### **Adding a Datafile**

```sql
ALTER TABLESPACE my_tablespace 
ADD DATAFILE '/u01/oradata/my_tablespace02.dbf' 
SIZE 100M AUTOEXTEND ON;
```

- Adds a new **datafile** to the tablespace.

#### **Autoextend ON/OFF**

```sql
ALTER DATABASE DATAFILE '/u01/oradata/my_tablespace.dbf' AUTOEXTEND ON NEXT 50M MAXSIZE 1G;
```

- Enables auto-extension for an existing **datafile**.

```sql
ALTER DATABASE DATAFILE '/u01/oradata/my_tablespace.dbf' AUTOEXTEND OFF;
```

- Disables auto-extension.

#### **Changing Tablespace Status**

```sql
ALTER TABLESPACE my_tablespace READ ONLY;
```

- Makes a tablespace **read-only**.

```sql
ALTER TABLESPACE my_tablespace READ WRITE;
```

- Makes a tablespace **writable**.

```sql
ALTER TABLESPACE my_tablespace OFFLINE;
```

- Takes the tablespace **offline** (not accessible).

```sql
ALTER TABLESPACE my_tablespace ONLINE;
```

- Brings the tablespace **back online**.

#### **Resizing a Datafile**

```sql
ALTER DATABASE DATAFILE '/u01/oradata/my_tablespace.dbf' RESIZE 500M;
```

- Resizes an existing **datafile**.

---

### **3. Dropping a Tablespace**

```sql
DROP TABLESPACE my_tablespace INCLUDING CONTENTS AND DATAFILES;
```

- `INCLUDING CONTENTS`: Deletes all objects inside the tablespace.
- `AND DATAFILES`: Removes the associated datafiles.

```sql
DROP TABLESPACE temp_space;
```

- Deletes a **temporary tablespace**.

---

### **4. Viewing Tablespace Information**

#### **Listing All Tablespaces**

```sql
SELECT tablespace_name, status, contents FROM dba_tablespaces;
```

- Shows **all tablespaces** and their status.

#### **Listing All Datafiles**

```sql
SELECT file_name, tablespace_name, bytes/1024/1024 AS size_MB FROM dba_data_files;
```

- Displays **datafiles** associated with tablespaces.

#### **Listing Temporary Tablespaces**

```sql
SELECT tablespace_name, file_name, bytes/1024/1024 AS size_MB FROM dba_temp_files;
```

- Lists **temporary tablespaces** and their files.

#### **Checking Free Space in Tablespaces**

```sql
SELECT tablespace_name, file_id, block_id, bytes/1024/1024 AS free_space_MB 
FROM dba_free_space 
ORDER BY tablespace_name;
```

- Displays **available free space** in each tablespace.

#### **Checking Tablespace Usage**

```sql
SELECT tablespace_name, 
       ROUND(SUM(bytes)/1024/1024,2) AS allocated_MB, 
       ROUND(MAX(bytes)/1024/1024,2) AS largest_free_extent_MB 
FROM dba_free_space 
GROUP BY tablespace_name;
```

- Shows **total space allocated and largest free extent**.

---

### **5. Managing Temporary Tablespaces**

#### **Assigning a Default Temporary Tablespace**

```sql
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE temp_space;
```

- Sets `temp_space` as the **default temporary tablespace**.

#### **Viewing Temporary Tablespace Usage**

```sql
SELECT tablespace_name, file_id, bytes/1024/1024 AS temp_used_MB 
FROM dba_temp_files;
```

- Displays **used temporary space**.

---

### **6. Managing Undo Tablespaces**

#### **Changing the Active Undo Tablespace**

```sql
ALTER SYSTEM SET UNDO_TABLESPACE = undo_space;
```

- Changes the **current active undo tablespace**.

#### **Viewing Undo Tablespace Information**

```sql
SELECT tablespace_name, file_name, bytes/1024/1024 AS size_MB FROM dba_data_files WHERE tablespace_name = 'UNDOTBS1';
```

- Lists **undo tablespaces** and their size.

---

### **7. Renaming a Tablespace**

```sql
ALTER TABLESPACE my_tablespace RENAME TO new_tablespace;
```

- Renames an **existing tablespace**.

---

### **8. Moving a Datafile**

```sql
ALTER DATABASE RENAME FILE '/u01/oradata/my_tablespace.dbf' TO '/u02/oradata/new_location.dbf';
```

- Moves a **datafile** to a new location.

---

### **9. Setting Tablespace Quotas**

#### **Granting Quota to a User**

```sql
ALTER USER scott QUOTA 50M ON my_tablespace;
```

- Limits **user storage** in a tablespace.

#### **Granting Unlimited Quota**

```sql
ALTER USER scott QUOTA UNLIMITED ON my_tablespace;
```

- Removes **storage restrictions** for a user.

---

### **10. Setting Default Tablespaces for Users**

```sql
ALTER USER scott DEFAULT TABLESPACE my_tablespace;
```

- Sets a **default tablespace** for a user.

---

### **11. Checking Tablespace Fragmentation**

```sql
SELECT tablespace_name, COUNT(*) AS extents 
FROM dba_extents 
GROUP BY tablespace_name 
ORDER BY tablespace_name;
```

- Shows **extent count** per tablespace (useful for fragmentation analysis).

---

### **12. Forcing a Checkpoint**

```sql
ALTER SYSTEM CHECKPOINT;
```

- Forces a **checkpoint** to flush dirty buffers to datafiles.

---

## **Conclusion**

These commands help manage **tablespaces efficiently** in **Oracle SQL*Plus**, covering **creation, modification, deletion, monitoring, and troubleshooting**. Let me know if you need additional details! 🚀