Alright Chikh Jbal, holder of the sword and king of SGA spells — let’s power up this **Oracle DB Admin cheat sheet** to God-tier. I’m talkin’ bold, rich formatting, real-world examples, and max clarity. This is your fancy deep-dive 🧠🔥.

---

# ⚔️ Oracle Database Administration – Full Power Mode

---

## 🗃️ 1. **Core Database Files Explained**

|File Type|Description|Example Path|Notes|
|---|---|---|---|
|**Control File**|Keeps track of the DB structure: DB name, SCN, redo logs, datafiles.|`/u01/oradata/ORCL/control01.ctl`|Crucial for startup. Backup required.|
|**Parameter File**|Stores DB initialization params.|`initORCL.ora` (PFILE) or `spfileORCL.ora` (SPFILE)|SPFILE is binary and server-managed.|
|**Datafile**|Contains actual data (tables, indexes).|`/u01/oradata/ORCL/users01.dbf`|Attached to a tablespace.|
|**Redo Log File**|Logs every change made to the DB (before commit).|`redo01.log`|Required for crash recovery.|
|**Archived Log File**|Backup of redo logs. Used in media recovery.|`arch_001.arc`|Only if ARCHIVELOG mode is on.|

> 💡 Tip: Always multiplex control and redo log files for safety.

---

## 🧠 2. **What Is an Instance?**

> 🔹 **Instance = Memory + Background Processes**  
> Starts before accessing datafiles.

---

## 🔮 3. **Instance Components: Deep Dive**

### 🔷 SGA (System Global Area)

|Subcomponent|Role|
|---|---|
|**Shared Pool**|Caches SQL, PL/SQL code, and dictionary data.|
|**Database Buffer Cache**|Holds data blocks read from disk.|
|**Redo Log Buffer**|Caches redo info before writing to disk.|
|**Large Pool**|Optional: Used for backup/restore, large sorts.|
|**Java Pool**|Stores Java classes and methods.|

```sql
-- View SGA size
SHOW SGA;
SELECT * FROM v$sga;
```

### 🧷 PGA (Program Global Area)

- Private memory for sessions (sorting, joins, etc.).
    
- One PGA per server process.

```sql
-- View PGA stats
SELECT * FROM v$pgastat;
```

---

## 🛠️ 4. **Oracle Background Processes**

|Process|Function|
|---|---|
|**DBWR**|Writes dirty buffers to datafiles.|
|**LGWR**|Writes redo log buffer to redo logs.|
|**CKPT**|Signals checkpoint: updates control/datafiles.|
|**SMON**|Performs crash recovery, coalesces free space.|
|**PMON**|Cleans up failed user processes.|
|**ARCn**|Copies redo logs to archive logs.|
|**MMAN**|Manages automatic memory management.|

```sql
-- Monitor background processes
SELECT * FROM v$bgprocess;
```

---

## 📊 5. **Oracle Views (Static vs Dynamic)**

### 🟢 Static Views (Metadata)

- **Prefix**: `DBA_`, `ALL_`, `USER_`
    
- Example: `DBA_TABLES`, `USER_OBJECTS`, `ALL_USERS`

### 🔴 Dynamic Views (Runtime Info)

- **Prefix**: `V$`, `GV$`
    
- Example: `V$INSTANCE`, `V$PROCESS`, `V$SESSION`

```sql
SELECT name, open_mode FROM v$database;
```

---

## 🧩 6. **PL/SQL Syntax – Functions & Procedures**

### ⚡ Function

```sql
CREATE OR REPLACE FUNCTION get_salary(emp_id IN NUMBER)
RETURN NUMBER IS
  sal NUMBER;
BEGIN
  SELECT salary INTO sal FROM employees WHERE employee_id = emp_id;
  RETURN sal;
END;
```

### ⚡ Procedure

```sql
CREATE OR REPLACE PROCEDURE raise_salary(emp_id IN NUMBER, percent IN NUMBER) IS
BEGIN
  UPDATE employees
  SET salary = salary + (salary * percent / 100)
  WHERE employee_id = emp_id;
END;
```

### Execution

```sql
BEGIN
  raise_salary(101, 10);
  DBMS_OUTPUT.PUT_LINE(get_salary(101));
END;
```

---

## 🧾 7. **Input / Output in SQL*Plus**

### Print

```sql
SET SERVEROUTPUT ON [SIZE 1000000 % For Buffer Size %];

DBMS_OUTPUT.PUT_LINE('Hello world!');
```

### Read

```sql
ACCEPT emp_id NUMBER PROMPT 'Enter employee ID: '
```

---

## 🔁 8. **Cursors in PL/SQL**

### Explicit Cursor

## 💡 Example Cursor Declaration (Common to All)

```sql
CURSOR c_emp IS
  SELECT employee_id, salary FROM employees;
```

---

## 🔁 1. **Basic Loop with FETCH (Manual Looping)**

You already nailed this one:

```sql
DECLARE
  CURSOR c_emp IS SELECT employee_id, salary FROM employees;
  rec c_emp%ROWTYPE;
BEGIN
  OPEN c_emp;
  LOOP
    FETCH c_emp INTO rec;
    EXIT WHEN c_emp%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE('ID: ' || rec.employee_id || ' Salary: ' || rec.salary);
  END LOOP;
  CLOSE c_emp;
END;
```

---

## 🔁 2. **FOR Loop with Implicit Cursor Handling (Auto OPEN/FETCH/CLOSE)**

This is the cleanest and most used style:

```sql
DECLARE
  CURSOR c_emp IS SELECT employee_id, salary FROM employees;
BEGIN
  FOR rec IN c_emp LOOP
    DBMS_OUTPUT.PUT_LINE('ID: ' || rec.employee_id || ' Salary: ' || rec.salary);
  END LOOP;
END;
```

> ✅ No need to open, fetch, or close manually — Oracle handles it for you like a personal assistant.

---

## 🔁 3. **WHILE Loop (Manual OPEN/FETCH/CLOSE with WHILE)**

More control than FOR loop, but less clean than #2:

```sql
DECLARE
  CURSOR c_emp IS SELECT employee_id, salary FROM employees;
  rec c_emp%ROWTYPE;
BEGIN
  OPEN c_emp;
  FETCH c_emp INTO rec;
  WHILE c_emp%FOUND LOOP
    DBMS_OUTPUT.PUT_LINE('ID: ' || rec.employee_id || ' Salary: ' || rec.salary);
    FETCH c_emp INTO rec;
  END LOOP;
  CLOSE c_emp;
END;
```

> 💬 Use when you want to **peek at values first** or do something before looping.

---

## 🔄 Comparison Table

|Loop Style|Needs `OPEN`/`CLOSE`?|Best For|
|---|---|---|
|`LOOP...EXIT WHEN`|✅ Yes|Full manual control|
|`FOR ... IN cursor`|❌ No|Quick, clean iteration|
|`WHILE cursor%FOUND`|✅ Yes|Conditional logic during fetch|

---

## 🚨 Bonus Tip: Cursor with Parameters

```sql
CURSOR c_emp (dep_id NUMBER) IS
  SELECT employee_id, salary FROM employees WHERE department_id = dep_id;

-- Call it:
FOR rec IN c_emp(60) LOOP
  DBMS_OUTPUT.PUT_LINE('ID: ' || rec.employee_id || ' Salary: ' || rec.salary);
END LOOP;
```

---

## 🚨 9. **Logs, Alerts & Flashback**

## 🌀 **1. Flashback: Rewind Time Without Backups**

Oracle's Flashback features let you undo human mistakes **WITHOUT restoring a backup**. Let’s break it down like magic scrolls 🧙‍♂️.

---

### 🌟 **FLASHBACK TABLE**

### ✅ Syntax:

```sql
FLASHBACK TABLE table_name
TO { SCN | TIMESTAMP | BEFORE DROP };
```

---

### 📌 **Examples**:

#### 💥 Undo a DROP:

```sql
FLASHBACK TABLE employees TO BEFORE DROP;
```

> 🧪 Table must be in the **recycle bin**. Enabled by default unless dropped with `PURGE`.

---

#### ⏳ Restore Table to Past State Using Timestamp:

```sql
FLASHBACK TABLE employees
TO TIMESTAMP TO_TIMESTAMP('2025-05-31 12:00:00', 'YYYY-MM-DD HH24:MI:SS');
```

> 🧠 This only works if **UNDO** data is still available for that time.

---

#### 🔢 Restore Table Using SCN (System Change Number):

```sql
FLASHBACK TABLE employees TO SCN 123456;
```

> You can get SCN using:

```sql
SELECT CURRENT_SCN FROM V$DATABASE;
```

---

### ⚠️ Requirements:

- `FLASHBACK TABLE` privilege.
    
- Row Movement must be enabled:

```sql
ALTER TABLE employees ENABLE ROW MOVEMENT;
```

---

### 🔄 **FLASHBACK DATABASE** — Travel Entire DB Back in Time

### ✅ Syntax:

```sql
FLASHBACK DATABASE TO { SCN | TIMESTAMP | RESTORE POINT };
```

---

### 📌 Examples:

#### ⏳ Flashback using timestamp:

```sql
FLASHBACK DATABASE TO TIMESTAMP TO_TIMESTAMP('2025-05-31 12:00:00', 'YYYY-MM-DD HH24:MI:SS');
```

#### 🔢 Flashback using SCN:

```sql
FLASHBACK DATABASE TO SCN 105789;
```

#### 🛑 Flashback to a Restore Point:

```sql
FLASHBACK DATABASE TO RESTORE POINT before_update;
```

> You must first create the restore point:

```sql
CREATE RESTORE POINT before_update GUARANTEE FLASHBACK DATABASE;
```

---

### 💣 Important Notes:

|Requirement|Description|
|---|---|
|🔄 Flashback Enabled|`ALTER DATABASE FLASHBACK ON;`|
|🔐 Archivelog Mode|Required to use flashback database|
|💾 Fast Recovery Area|Must be set with enough space|
|🛡️ MOUNT Mode Only|You must shutdown and `STARTUP MOUNT` before running `FLASHBACK DATABASE`|

---

## 🧠 Summary Cheat Sheet

| Feature              | Target       | Usage Example                           |
| -------------------- | ------------ | --------------------------------------- |
| `FLASHBACK TABLE`    | One table    | `FLASHBACK TABLE emp TO BEFORE DROP;`   |
| `FLASHBACK DATABASE` | Full DB      | `FLASHBACK DATABASE TO SCN 12345;`      |
| `TO SCN`             | Any          | Precise number-based rollback           |
| `TO TIMESTAMP`       | Any          | Human-readable datetime                 |
| `TO RESTORE POINT`   | Full DB      | Restore to manually created safe point  |
| `Alert Log`          | Full DB info | Tracks all DB-level activities & errors |

---

## 🔒 **AUDIT in Oracle** — The Ultimate Spy Tool 🕶️

Oracle's **AUDIT** command lets you **track who did what, when, and how** inside the database. Useful for **security, compliance, and catching sus behavior** 👀.

---

## 🔹 **Basic Syntax**

```sql
AUDIT action [BY user] [BY SESSION | BY ACCESS] [WHENEVER [NOT] SUCCESSFUL];
```

---

## 🔹 **Keywords Explained**

|Keyword|Meaning|
|---|---|
|`action`|The operation to audit (e.g., SELECT, INSERT, DELETE)|
|`BY user`|Limit auditing to a specific user|
|`BY SESSION`|Logs 1 record per session|
|`BY ACCESS`|Logs every access attempt individually|
|`WHENEVER SUCCESSFUL`|Only log successful attempts|
|`WHENEVER NOT SUCCESSFUL`|Only log failures|

---

## ⚡ **Examples:**

### ✅ 1. Audit SELECT on a specific table:

```sql
AUDIT SELECT ON employees;
```

### ✅ 2. Audit INSERT, DELETE, UPDATE on a table:

```sql
AUDIT INSERT, UPDATE, DELETE ON hr.departments;
```

### ✅ 3. Audit actions only when performed by a user:

```sql
AUDIT SELECT ON hr.employees BY scott;
```

### ✅ 4. Audit a system privilege:

```sql
AUDIT CREATE TABLE;
```

### ✅ 5. Audit only successful logins:

```sql
AUDIT CREATE SESSION WHENEVER SUCCESSFUL;
```

### ✅ 6. Audit every single SELECT:

```sql
AUDIT SELECT TABLE BY ACCESS;
```

> 🧠 `SELECT TABLE` = all `SELECT` operations on any table in the schema.

---

## 🧽 **Undo It (Clean Up the Spy Logs)**

### ❌ Turn off auditing:

```sql
NOAUDIT SELECT ON employees;
NOAUDIT CREATE SESSION;
```

---

## 📦 **Where Is the Audit Info Stored?**

Depends on audit mode:

|Mode|Audit Data Stored In|
|---|---|
|Traditional Auditing|`DBA_AUDIT_TRAIL` view|
|Fine-Grained Auditing|`DBA_FGA_AUDIT_TRAIL` view|
|Unified Auditing|`UNIFIED_AUDIT_TRAIL` view|

> Use:

```sql
SELECT * FROM DBA_AUDIT_TRAIL;
```

---

## ⚙️ **Check if Auditing Is Enabled**

```sql
SHOW PARAMETER audit_trail;
```

> Should return: `DB` or `DB,EXTENDED` or `XML` if it’s on.

---

## 🚀 Enable Auditing (if not already enabled):

```sql
ALTER SYSTEM SET audit_trail=DB SCOPE=SPFILE;
-- Then restart the DB for it to take effect.
```

---
## 🧱 **Oracle Instance Lifecycle: The Levels**

Oracle DB startup goes through 3 **main phases**:

> 🛠️ `NOMOUNT` → 🔐 `MOUNT` → 🟢 `OPEN`

Each level unlocks more capabilities. Here’s the breakdown:

---

## 🌑 1. **STARTUP NOMOUNT**

### 🔧 Syntax:

```sql
STARTUP NOMOUNT;
```
### 🧠 What Happens:

- Instance is created
    
- Oracle reads **parameter file (PFILE/SPFILE)**
    
- Allocates memory (SGA & PGA)
    
- Starts background processes (DBWn, LGWR, SMON, etc.)

### 🔍 What You Can Do:

|✅ Allowed Actions|Notes|
|---|---|
|CREATE DATABASE|Used during first-time DB creation|
|RESTORE CONTROL FILE|During recovery scenarios|
|CREATE CONTROLFILE|Re-create if it’s missing or corrupted|

---

## 🗄️ 2. **STARTUP MOUNT**

### 🔧 Syntax:

```sql
STARTUP MOUNT;
```

### 🧠 What Happens:

- Control files are read
    
- DB is associated with the instance
    
- No datafiles are accessed yet

### 🔍 What You Can Do:

|✅ Allowed Actions|Notes|
|---|---|
|Perform full database recovery|`RECOVER DATABASE`|
|Enable/disable ARCHIVELOG|`ALTER DATABASE ARCHIVELOG;`|
|Rename/Relocate datafiles|Before OPEN phase|
|View metadata info|From control files (like tablespace names)|

---

## 🟢 3. **STARTUP OPEN**

### 🔧 Syntax:

```sql
STARTUP;
-- or
STARTUP OPEN;
```

### 🧠 What Happens:

- Datafiles and redo logs are opened
    
- The DB becomes accessible to users
    
- SMON checks for recovery needs and consistency
    

### 🔍 What You Can Do:

|✅ Allowed Actions|Notes|
|---|---|
|Normal user activity|SELECT, INSERT, etc.|
|Run DDL and DML|Full DB usage|
|Backup/Export DB|RMAN, Data Pump|

---

## 💣 BONUS: **RESTRICTED Mode**

> Only users with `RESTRICTED SESSION` privilege can connect.

```sql
STARTUP RESTRICT;
```

---

## 🧯 Shutdown Modes (Clean Exits)

### 🔻 Syntax:

```sql
SHUTDOWN [NORMAL | IMMEDIATE | TRANSACTIONAL | ABORT];
```

|Mode|Description|
|---|---|
|**NORMAL**|Waits for users to disconnect. No new connections.|
|**IMMEDIATE**|Rolls back uncommitted transactions. Faster. Most used.|
|**TRANSACTIONAL**|Waits for active transactions to finish, then shuts down.|
|**ABORT**|💀 Emergency kill. No rollback. DB will need recovery on next startup.|

---

## 🔄 Lifecycle Flow Chart

```text
STARTUP SHUTDOWN
   |
   ↓
STARTUP NOMOUNT
   |
   ↓
STARTUP MOUNT
   |
   ↓
STARTUP OPEN
```

---

## ⚙️ Extra Commands & Checks

### 🧩 Check Instance Status:

```sql
SELECT STATUS FROM V$INSTANCE;
```

### 💬 Check Open Mode:

```sql
SELECT OPEN_MODE FROM V$DATABASE;
```

---

## 🧠 TL;DR Cheat Sheet

|Level|Memory|Control Files|Datafiles|User Access|Usage|
|---|---|---|---|---|---|
|NOMOUNT|✅|❌|❌|❌|DB creation, recovery|
|MOUNT|✅|✅|❌|❌|Recovery, rename files|
|OPEN|✅|✅|✅|✅|Normal ops|

---

## 🏗️ 10. **Tablespaces & Storage Concepts**

### Storage Hierarchy:

```
Tablespace > Segment > Extent > Block [TSEB]
```
### Famous Tablespaces:

- `SYSTEM`: Core Oracle schema objects.
    
- `SYSAUX`: Auxiliary components (like AWR).
    
- `TEMP`: Temporary operations.
    
- `UNDOTBS1`: Undo data.
    
- `USERS`: Default tablespace for users.
### Calculate Space

```sql
-- Free Space
SELECT tablespace_name, SUM(bytes)/1024/1024 AS free_mb
FROM dba_free_space GROUP BY tablespace_name;

-- Used Space
SELECT tablespace_name, SUM(bytes)/1024/1024 AS used_mb
FROM dba_data_files GROUP BY tablespace_name;
```

---

### Create / Alter / Drop Tablespace

```sql
-- Create
CREATE TABLESPACE data_ts DATAFILE 'data01.dbf' SIZE 100M;

-- Add datafile
ALTER TABLESPACE data_ts ADD DATAFILE 'data02.dbf' SIZE 50M;

-- Drop
DROP TABLESPACE data_ts INCLUDING CONTENTS AND DATAFILES;
```

---

### Change Default Tablespace

```sql
ALTER DATABASE DEFAULT TABLESPACE users;
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE temp;
```

---

## 👥 11. **Users, Roles, and Privileges**

### Famous Users

- `SYS`: Root DBA
    
- `SYSTEM`: Secondary admin
    
- `HR`, `SCOTT`: Sample schemas
    

### Create Role & Assign

```sql
CREATE ROLE hr_read;
GRANT SELECT ON employees TO hr_read;
GRANT hr_read TO some_user;
```

### Grant Privileges

```sql
GRANT CREATE SESSION TO some_user;
GRANT SELECT, INSERT ON employees TO some_user;
```
## 💠 Two Main Categories of Privileges in Oracle

|Category|Description|
|---|---|
|**System Privileges**|Permissions to perform actions on the _entire DB_ (e.g., create users, create tables)|
|**Object Privileges**|Permissions to perform actions on _specific objects_ (e.g., select from a table, execute a procedure)|

---

## 🔹 1. **System Privileges** 🏛️

These give you the **power over the whole database environment**.

### 🔧 Examples:

|Privilege|What It Allows|
|---|---|
|`CREATE SESSION`|Login to the DB|
|`CREATE TABLE`|Create tables in your schema|
|`CREATE USER`|Create new users|
|`ALTER SYSTEM`|Change DB-level configs|
|`DROP ANY TABLE`|Drop any table in the DB (scary 👀)|
|`GRANT ANY PRIVILEGE`|Give any privilege to anyone|
|`CREATE ANY VIEW`|Create views in any schema|
|`EXECUTE ANY PROCEDURE`|Execute any stored procedure|

> 🔥 Use with caution — these are like admin commands.

### ✅ Grant System Privilege:

```sql
GRANT CREATE TABLE TO scott;
```

### ❌ Revoke System Privilege:

```sql
REVOKE CREATE TABLE FROM scott;
```

---

## 🔸 2. **Object Privileges** 📦

These are granted **on specific database objects** like tables, views, procedures, etc.

### 🔧 Examples:

|Object Type|Privileges You Can Grant|
|---|---|
|**TABLE**|`SELECT`, `INSERT`, `UPDATE`, `DELETE`, `REFERENCES`|
|**VIEW**|`SELECT`, `UPDATE`, `INSERT`, `DELETE`|
|**SEQUENCE**|`SELECT`, `ALTER`|
|**PROCEDURE/FUNCTION**|`EXECUTE`|
|**PACKAGE**|`EXECUTE`|

### ✅ Grant Object Privilege:

```sql
GRANT SELECT, INSERT ON employees TO scott;
```

### ❌ Revoke Object Privilege:

```sql
REVOKE INSERT ON employees FROM scott;
```

---

## 💎 3. **WITH GRANT OPTION** & **WITH ADMIN OPTION**

### 👉 `WITH GRANT OPTION`

> Lets the user **pass on** object privileges to others.

```sql
GRANT SELECT ON employees TO ali WITH GRANT OPTION;
```

Now `ali` can grant `SELECT` on `employees` to others.

---

### 👉 `WITH ADMIN OPTION`

> Same concept but for **system privileges** and **roles**.

```sql
GRANT CREATE USER TO admin1 WITH ADMIN OPTION;
```

---

## 🧙‍♂️ BONUS: Roles

### ✅ Create Role:

```sql
CREATE ROLE hr_manager;
```

### ✅ Grant Privileges to Role:

```sql
GRANT SELECT, INSERT ON employees TO hr_manager;
GRANT CREATE SESSION TO hr_manager;
```

### ✅ Grant Role to User:

```sql
GRANT hr_manager TO salma;
```

---

## 🔍 Check What You Got

### See your own privileges:

```sql
SELECT * FROM USER_SYS_PRIVS;
SELECT * FROM USER_TAB_PRIVS;
```

### See all privileges:

```sql
SELECT * FROM DBA_SYS_PRIVS;
SELECT * FROM DBA_TAB_PRIVS;
```

---

## 🧠 TL;DR Table

|Type|Applies To|Examples|
|---|---|---|
|System Privilege|Whole DB system|`CREATE SESSION`, `ALTER USER`|
|Object Privilege|Specific objects|`SELECT ON emp`, `EXECUTE ON pkg_utils`|

---

## ⚙️ **What is a Trigger in Oracle?**

> A **Trigger** is a **PL/SQL block** that automatically **executes in response to DML events** (INSERT, UPDATE, DELETE), DDL events (like `CREATE TABLE`), or database events (like login, logoff, etc.).

---

## 🎯 **Types of Triggers**

|Type|Fires When...|Example Use Case|
|---|---|---|
|**DML Trigger**|INSERT, UPDATE, DELETE on a table/view|Auto-update a timestamp on row change|
|**DDL Trigger**|CREATE, DROP, ALTER, etc.|Audit schema changes|
|**LOGON/LOGOFF**|User connects or disconnects|Track login times|
|**INSTEAD OF**|For views (since you can’t normally update views)|Enable updates on complex views|
|**Compound Trigger**|Combines multiple DML timings in one trigger|Reduce context switches|

---

## 🧪 DML Trigger Syntax

### ✅ Example: Trigger before INSERT

```sql
CREATE OR REPLACE TRIGGER trg_emp_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  :NEW.hire_date := SYSDATE;
END;
```
### The Big 3 Oracle DML Operations for triggers:

| Operation   | Trigger Condition Check | What it Means                     |
| ----------- | ----------------------- | --------------------------------- |
| `INSERTING` | `IF INSERTING THEN`     | A new row is being added          |
| `UPDATING`  | `ELSIF UPDATING THEN`   | An existing row is being modified |
| `DELETING`  | `ELSIF DELETING THEN`   | A row is being removed            |

---

### Bonus: Other Oracle Trigger Event Types (not in your code but good to know)

- **`BEFORE INSERT`, `AFTER INSERT`** — when row is inserted, before or after
    
- **`BEFORE UPDATE`, `AFTER UPDATE`** — when row is updated
    
- **`BEFORE DELETE`, `AFTER DELETE`** — when row is deleted
    
- **`INSTEAD OF`** — used on views for custom DML handling
    
- **`BEFORE STATEMENT` and `AFTER STATEMENT`** — triggers that fire once per SQL statement, not per row
    

### 🧠 Explanation:

|Part|Meaning|
|---|---|
|`CREATE OR REPLACE`|Creates or updates the trigger|
|`BEFORE INSERT`|Runs **before** an insert operation|
|`ON employees`|Attached to the `employees` table|
|`FOR EACH ROW`|Runs for **every row** being inserted|
|`:NEW.hire_date`|Refers to the new value going into the `hire_date` field|
|`SYSDATE`|Oracle’s system date/time|

---

## 🔁 Types of Timing

|Timing|Description|
|---|---|
|**BEFORE**|Trigger fires **before** the event (e.g. before insert)|
|**AFTER**|Trigger fires **after** the event|
|**INSTEAD OF**|Used **on views** to simulate DML|

---

## 💡 Example: AFTER UPDATE Trigger

```sql
CREATE OR REPLACE TRIGGER trg_salary_audit
AFTER UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
  INSERT INTO audit_log (emp_id, old_salary, new_salary, changed_on)
  VALUES (:OLD.employee_id, :OLD.salary, :NEW.salary, SYSDATE);
END;
```

> This logs any salary update to an `audit_log` table.

---

## 🚀 INSTEAD OF Trigger (for Views)

```sql
CREATE OR REPLACE TRIGGER trg_upd_emp_view
INSTEAD OF UPDATE ON emp_view
FOR EACH ROW
BEGIN
  UPDATE employees
  SET salary = :NEW.salary
  WHERE employee_id = :OLD.employee_id;
END;
```

---

## 🧱 Compound Trigger

Multiple timings in one trigger (like `BEFORE STATEMENT`, `BEFORE ROW`, etc.)

```sql
CREATE OR REPLACE TRIGGER trg_comp_example
FOR INSERT ON employees
COMPOUND TRIGGER

  BEFORE STATEMENT IS
  BEGIN
    DBMS_OUTPUT.PUT_LINE('Before statement...');
  END BEFORE STATEMENT;

  BEFORE EACH ROW IS
  BEGIN
    :NEW.hire_date := SYSDATE;
  END BEFORE EACH ROW;

  AFTER EACH ROW IS
  BEGIN
    DBMS_OUTPUT.PUT_LINE('Row inserted!');
  END AFTER EACH ROW;

  AFTER STATEMENT IS
  BEGIN
    DBMS_OUTPUT.PUT_LINE('After statement.');
  END AFTER STATEMENT;

END trg_comp_example;
```

---

## 🔍 Trigger Events Reference

|Keyword|Description|
|---|---|
|`:NEW`|New value being inserted/updated|
|`:OLD`|Old value being updated/deleted|

---

## ❌ Disable / Drop Trigger

```sql
ALTER TRIGGER trg_emp_insert DISABLE;
DROP TRIGGER trg_emp_insert;
```

---

## ✅ Check Existing Triggers

```sql
SELECT trigger_name, status, table_name
FROM user_triggers;
```

---

## 🔐 Security Note:

Triggers run with the privileges of the user who **owns** the trigger, **not** the one who executes the DML action. That means they can secretly do stuff behind the scenes.

---

## 🧠 TL;DR Summary

|Trigger Type|Best For|Example Action|
|---|---|---|
|BEFORE INSERT|Validating or defaulting data|Add `SYSDATE` to `hire_date`|
|AFTER UPDATE|Auditing or syncing data|Insert into audit table|
|INSTEAD OF|Views that can’t be updated normally|Allow updates to view|
|COMPOUND|Grouping all trigger timings|Cleaner and faster code|

---

## 📚 Bonus Tip: Data Dictionary

- Built-in collection of tables/views containing metadata.
    
- Example: `DBA_OBJECTS`, `DBA_USERS`, `DBA_TABLES`
    
- Stored in `SYSTEM` tablespace.

---
## 💡 **What is a PROFILE?**

A **Profile** in Oracle defines a set of **resource limits** and **password management rules**.  
You assign profiles to users to **control their behavior**, like session timeouts, login attempts, or even password complexity.

---

## ⚙️ **CREATE PROFILE – Syntax & Options**

```sql
CREATE PROFILE profile_name
  LIMIT
    [RESOURCE_NAME {integer | UNLIMITED | DEFAULT}]
    [PASSWORD_PARAMETER {value | UNLIMITED | DEFAULT}]
;
```

### 🧠 Full Example:

```sql
CREATE PROFILE strict_profile
LIMIT
  SESSIONS_PER_USER     2
  CPU_PER_SESSION       10000
  CONNECT_TIME          60
  FAILED_LOGIN_ATTEMPTS 3
  PASSWORD_LOCK_TIME    1
  PASSWORD_LIFE_TIME    30;
```

---

## 🔹 **Resource Parameters** (for sessions/resources)

|Parameter|Description|
|---|---|
|`SESSIONS_PER_USER`|Max sessions per user|
|`CPU_PER_SESSION`|CPU time limit per session (hundredths of seconds)|
|`CPU_PER_CALL`|CPU time per call (a logical unit of work)|
|`CONNECT_TIME`|Minutes a session can stay connected|
|`IDLE_TIME`|Minutes of inactivity before session killed|
|`LOGICAL_READS_PER_SESSION`|Total block reads (buffer cache)|
|`PRIVATE_SGA`|Private space in SGA (in bytes)|

> ✅ Example:

```sql
CREATE PROFILE perf_limited LIMIT CONNECT_TIME 30 IDLE_TIME 10;
```

---

## 🔸 **Password Parameters** (for password policy enforcement)

|Parameter|Description|
|---|---|
|`FAILED_LOGIN_ATTEMPTS`|Number of failed logins before lock|
|`PASSWORD_LIFE_TIME`|Days before password expires|
|`PASSWORD_REUSE_TIME`|Days before a reused password is allowed|
|`PASSWORD_REUSE_MAX`|Number of changes before reuse allowed|
|`PASSWORD_LOCK_TIME`|Days account remains locked|
|`PASSWORD_GRACE_TIME`|Days after password expiry before lock|
|`PASSWORD_VERIFY_FUNCTION`|Function that validates password strength|

> ✅ Example:

```sql
CREATE PROFILE secure_user LIMIT
  FAILED_LOGIN_ATTEMPTS 5
  PASSWORD_LOCK_TIME 1
  PASSWORD_LIFE_TIME 30;
```

> ✅ With password complexity function:

```sql
CREATE PROFILE ultra_secure LIMIT
  PASSWORD_VERIFY_FUNCTION verify_function_name;
```

---

## 🔧 **ALTER PROFILE – Change an existing profile**

```sql
ALTER PROFILE profile_name LIMIT
  CONNECT_TIME 20
  IDLE_TIME 5;
```

---

## 🗑️ **DROP PROFILE**

```sql
DROP PROFILE profile_name CASCADE;
```

> 🔥 `CASCADE` is used to force-drop the profile even if users are using it.

---

## 👑 **Assign a Profile to a User**

```sql
ALTER USER username PROFILE profile_name;
```

---

## 🔍 **View Existing Profiles**

```sql
SELECT * FROM DBA_PROFILES WHERE PROFILE = 'DEFAULT';
```

---

## 📚 Common Profiles

|Profile|Description|
|---|---|
|`DEFAULT`|Automatically assigned if not set|
|Custom profiles|You can make your own based on needs|

---

## 🔒 TL;DR Cheat Sheet

|Action|Syntax Example|
|---|---|
|Create Profile|`CREATE PROFILE cool LIMIT CONNECT_TIME 30;`|
|Alter Profile|`ALTER PROFILE cool LIMIT IDLE_TIME 10;`|
|Drop Profile|`DROP PROFILE cool CASCADE;`|
|Assign to User|`ALTER USER chikh PROFILE cool;`|
