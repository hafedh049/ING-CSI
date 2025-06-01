![[Pasted image 20250531140405.png]]
![[Pasted image 20250531140413.png]]
![[Pasted image 20250531140418.png]]
![[Pasted image 20250601113027.png]]

---


# Partie I

#### **1. Soit la commande suivante exécutée en tant qu’administrateur : DROP PROFILE PROFIL_EXAM CASCADE;  

Quelle est l’affirmation correcte ?**

- **A.** Tous les utilisateurs ayant le profil PROFIL_EXAM n’en auront plus de profil.
- **B.** Tous les utilisateurs ayant le profil PROFIL_EXAM auront comme profil USERS.
- **C.** Tous les utilisateurs ayant le profil PROFIL_EXAM auront comme profil CASCADE.
- **D.** Aucune de ces réponses.

**Correction:**  
The `DROP PROFILE ... CASCADE` command in Oracle SQL removes the specified profile (PROFIL_EXAM) and disassociates it from all users who were assigned to it. When a profile is dropped with the `CASCADE` option, users who were assigned that profile will no longer have a profile assigned—they are not reassigned to another profile like USERS or CASCADE. Therefore, the correct answer is:  
**A. Tous les utilisateurs ayant le profil PROFIL_EXAM n’en auront plus de profil.**

---

#### **2. Comment on peut surveiller l’insertion au niveau de la table EMPLOYEES propriétaire de l’utilisateur HR pour chaque instruction lancée ?**

- **A.** AUDIT INSERT ON HR.EMPLOYEES BY SESSION
- **B.** AUDIT INSERT ON HR.EMPLOYEES BY ACCESS
- **C.** AUDIT INSERT ON EMPLOYEES BY SESSION
- **D.** AUDIT INSERT ON EMPLOYEES BY HR BY ACCESS

##### **Correction:**  
To monitor insertions on the `EMPLOYEES` table owned by the `HR` user, you need to enable auditing. The correct syntax must specify the schema (HR) and the table (EMPLOYEES). Additionally, to audit each insert operation (per statement), you use `BY ACCESS` (as opposed to `BY SESSION`, which groups actions in a session). The correct answer is:  
**B. AUDIT INSERT ON HR.EMPLOYEES BY ACCESS**

---

#### **3. Suite à l’exécution des instructions suivantes, choisir l’affirmation correcte :**

```SQL
SQL> connect /as sys seguimos
Connected.
SQL> GRANT create session to user01 with admin option;
Grant succeeded.
SQL> connect user01/user01;
Connected.
SQL> GRANT create session to user02;
Grant succeeded.
SQL> connect /as sysdba
Connected.
SQL> REVOKE create session from user01;
Revoke succeeded.
```

- **A.** Autre que l’administrateur, seul l’utilisateur user01 a le droit de créer une session.
- **B.** Autre que l’administrateur, seul l’utilisateur user02 a le droit de créer une session.
- **C.** Autre que l’administrateur, aucun des utilisateurs user01 et user02 n’a le droit de créer une session.
- **D.** Aucune de ces réponses.

##### **Correction:**  
- The `sysdba` grants `CREATE SESSION` to `user01` with the `WITH ADMIN OPTION`, allowing `user01` to grant this privilege to others.
- `user01` then grants `CREATE SESSION` to `user02`.
- When `sysdba` revokes `CREATE SESSION` from `user01`, the `WITH ADMIN OPTION` means that the revoke does not cascade to `user02` (since `ADMIN OPTION` privileges are independent). Thus, `user02` retains the `CREATE SESSION` privilege, but `user01` no longer has it.

The correct answer is:  
**B. Autre que l’administrateur, seul l’utilisateur user02 a le droit de créer une session.**

---

#### **4. D’après les instructions dans la figure 4, choisir l’affirmation correcte :**

```SQL
SQL> connect /as sysdba
Connected.
SQL> GRANT create session to user01 with admin option;
Grant succeeded.
SQL> connect user01/user01;
Connected.
SQL> GRANT create session to user02;
Grant succeeded.
```

- **A.** Autre que l’administrateur, les deux utilisateurs user01 et user02 peuvent déléguer le privilège ‘Create Session’.
- **B.** Autre que l’administrateur, les deux utilisateurs user01 et user02 ont le droit de créer une session.
- **C.** Autre que l’administrateur, aucun utilisateur ne peut se connecter.
- **D.** Aucune de ces réponses.

##### **Correction:**  
- `sysdba` grants `CREATE SESSION` to `user01` with `ADMIN OPTION`, so `user01` can create a session and delegate this privilege.
- `user01` grants `CREATE SESSION` to `user02`, so `user02` can also create a session. However, `user02` does not have the `ADMIN OPTION`, so they cannot delegate the privilege further.

The question asks about the ability to create a session, not delegate. Both `user01` and `user02` can create sessions. The correct answer is:  
**B.** **Autre que l’administrateur, les deux utilisateurs user01 et user02 ont le droit de créer une session.**

---

#### **5. Choisir la bonne instruction qui consiste à permettre à l’utilisateur SCOTT de déléguer le privilège « BACKUP ANY TABLE » à SCOTT.**

- **A.** GRANT BACKUP ANY TABLE TO SCOTT WITH GRANT OPTION;
- **B.** GRANT BACKUP ANY TABLE TO SCOTT WITH ADMIN OPTION;
- **C.** GRANT BACKUP ANY TABLE TO SCOTT;
- **D.** REVOKE BACKUP ANY TABLE FROM SCOTT;

##### **Correction:**  
To allow `SCOTT` to delegate the `BACKUP ANY TABLE` privilege to others, the `WITH ADMIN OPTION` or `WITH GRANT OPTION` is required. In Oracle, for system privileges like `BACKUP ANY TABLE`, the correct syntax is `WITH ADMIN OPTION`. The correct answer is:  
**B. GRANT BACKUP ANY TABLE TO SCOTT WITH ADMIN OPTION;**

---

#### **6. Choisir l’affirmation correcte. Suite à l’exécution des instructions suivantes, que peut faire un administrateur ?**

```SQL
SQL> connect /as sysdba
Connected.
SQL> show parameter audit_trail;
NAME              TYPE        VALUE
audit_trail       string      DB_EXTENDED
SQL> Alter system set audit_trail=OS scope=spfile;
System altered.
SQL> audit create table by hr by access;
Audit succeeded.
```

- **A.** Auditer les instructions de création de tables effectuées par tous les utilisateurs.
- **B.** Auditer toutes les instructions effectuées par tous les utilisateurs.
- **C.** Consulter les entrées d’audit à partir de l’utilisateur ‘hr’.
- **D.** Consulter les entrées d’audit à partir du fichier système d’exploitation.

##### **Correction:**  
- The `audit_trail` is initially set to `DB_EXTENDED`, meaning audit records are stored in the database.
- The `ALTER SYSTEM SET audit_trail=OS` changes the audit trail to write to the operating system (but requires a restart since it’s in the `spfile`).
- The `AUDIT CREATE TABLE BY HR BY ACCESS` audits table creation by the `HR` user, with records written per operation (`BY ACCESS`).

Since the audit trail is set to `OS` (though not yet active until a restart), the administrator can eventually consult audit entries from the OS. However, at the moment of execution, the audit records are still in the database (`DB_EXTENDED`). The question asks what the admin *can* do, and the most relevant action post-audit setup is to consult the audit entries for `HR`. The correct answer is:  
**C. Consulter les entrées d’audit à partir de l’utilisateur ‘hr’.**

---

# Partie II

### 1. Créer un profil PROFILE_EX ayant les limites suivantes : (1.5 pts)
- Le temps d’inactivité est limité à 8 heures,
- Le temps de vie du mot de passe est un mois,
- La durée de connexions simultanées est 2,
- Le nombre sera verrouillé pendant une semaine après 3 tentatives de connexions.

```sql
CREATE PROFILE PROFILE_EX LIMIT
    IDLE_TIME 480                  -- 8 hours (8 * 60 minutes)
    PASSWORD_LIFE_TIME 30          -- 1 month (30 days)
    SESSIONS_PER_USER 2            -- Max 2 simultaneous connections
    FAILED_LOGIN_ATTEMPTS 3        -- Lock after 3 failed attempts
    PASSWORD_LOCK_TIME 7;          -- Lock for 7 days
```

---

### 2. Créer un rôle ROLE_EX ayant : (1 pt)
- Le privilège de connexion à la base de données avec la possibilité de le transmettre,
- Les privilèges de lecture et d’insertion sur la table EMPLOYEES de l’utilisateur HR,
- Le RESOURCE.

```sql
-- Create the role
CREATE ROLE ROLE_EX;

-- Grant CREATE SESSION with ADMIN OPTION (to allow transmission)
GRANT CREATE SESSION TO ROLE_EX WITH ADMIN OPTION;

-- Grant SELECT and INSERT on HR.EMPLOYEES
GRANT SELECT, INSERT ON HR.EMPLOYEES TO ROLE_EX;

-- Grant RESOURCE role
GRANT RESOURCE TO ROLE_EX;
```

---

### 3. Créer deux utilisateurs : (1.5 pts)
#### A/ MANAGER_EX avec
- un mot de passe manager_ex,
- TBL_TEK comme tablespace par défaut,
- TEMP_TEK comme tablespace temporaire,
- le rôle ROLE_EX et le profil PROFILE_EX.

#### B/ USER_EX avec
- un mot de passe user_ex,
- TRI_TEK comme tablespace par défaut,
- TEMP_TEK comme tablespace temporaire,
- le rôle ROLE_EX et le profil PROFILE_EX.

```sql
-- Create MANAGER_EX
CREATE USER MANAGER_EX
    IDENTIFIED BY manager_ex
    DEFAULT TABLESPACE TBL_TEK
    TEMPORARY TABLESPACE TEMP_TEK
    PROFILE PROFILE_EX;

GRANT ROLE_EX TO MANAGER_EX;

-- Create USER_EX
CREATE USER USER_EX
    IDENTIFIED BY user_ex
    DEFAULT TABLESPACE TRI_TEK
    TEMPORARY TABLESPACE TEMP_TEK
    PROFILE PROFILE_EX;

GRANT ROLE_EX TO USER_EX;
```

---

### 4. La société cherche à auditer les activités de la base de données (0.5 pt)
#### a) Visualiser le paramètre d’audit de base de données (0.5 pt)
```sql
SHOW PARAMETER AUDIT_TRAIL;
```

#### b) Auditer les connexions à la base de données (0.5 pt)
```sql
AUDIT SESSION;
```

#### c) Auditer tous les select, insert, update et delete sur la table CLIENT de l’utilisateur MANAGER_EX par session en cas de succès (1 pt)
```sql
AUDIT SELECT, INSERT, UPDATE, DELETE ON MANAGER_EX.CLIENT
    BY SESSION WHENEVER SUCCESSFUL;
```

#### d) Donner la commande permettant de visualiser les entrées d’audit (1 pt)
```sql
SELECT USERNAME, ACTION_NAME, TIMESTAMP
FROM DBA_AUDIT_TRAIL;
```

#### e) Désactiver l’audit des instructions select sur la table CLIENT de l’utilisateur MANAGER_EX (1 pt)
```sql
NOAUDIT SELECT ON MANAGER_EX.CLIENT;
```

#### f) Donner les commandes suivantes : (1 pt)
- **SQL> audit select on hr.jobs by access whenever not successful;**
- **SQL> audit select on hr.jobs by access whenever successful;**
- **SQL> select object_name, sel, up from dba_obj_audit_opts where object_name = 'JOBS';**

```SQL
OBJECT_NAME  SEL  UP
----------- ---- ----
JOBS        A/A  -/-
```

---

### Additional Information from the Document:
- **SCN=200333 qui a été récupéré avant l’exécution d’une instruction de modification des données de la table Etudiants (update Etudiants set Date_ins = sysdate; commit;).**
- **NB : Soit la structure de la table etudiants:**
  - Id_etudiant
  - Nom_etudiant
  - Prenom_etudiant
  - Date_ins

----

- ✅ **Question 5**: Auditing modified data
    
- ✅ **Question 6**: `LOCK_USER` procedure
    
- ✅ **Question 7**: `LOCK_ALL_USERS` procedure using the first one
    
- ✅ **Annex**: Templates for tablespace, profile, user creation, and audit
    

---

### 💾 **Complete SQL Script**

```sql
-- ===========================
-- 🔐 Question 5: Enable Audit
-- ===========================
-- Track modifications (INSERT, UPDATE, DELETE) on a specific table
AUDIT INSERT, UPDATE, DELETE
ON your_schema.your_table
BY ACCESS
WHENEVER SUCCESSFUL;


-- ===========================
-- 🧑‍💻 Question 6: LOCK_USER
-- ===========================
CREATE OR REPLACE PROCEDURE LOCK_USER(p_username IN VARCHAR2) IS
BEGIN
   EXECUTE IMMEDIATE 'ALTER USER "' || p_username || '" ACCOUNT LOCK';
EXCEPTION
   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Error locking user ' || p_username || ': ' || SQLERRM);
END;
/


-- ===============================
-- 👥 Question 7: LOCK_ALL_USERS
-- ===============================
CREATE OR REPLACE PROCEDURE LOCK_ALL_USERS IS
BEGIN
   FOR usr IN (
      SELECT grantee 
      FROM dba_role_privs 
      WHERE granted_role = 'DBA'
   ) LOOP
      LOCK_USER(usr.grantee);
   END LOOP;

   -- Lock SYS explicitly (not recommended in production)
   LOCK_USER('SYS');
EXCEPTION
   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/


-- ==================
-- 📦 ANNEX Snippets
-- ==================

-- 1. CREATE TABLESPACE
CREATE TABLESPACE my_tablespace
DATAFILE 'my_file.dbf' SIZE 100M 
AUTOEXTEND ON NEXT 50M 
MAXSIZE UNLIMITED;

-- 2. CREATE USER
CREATE USER my_user IDENTIFIED BY my_password
DEFAULT TABLESPACE my_tablespace
TEMPORARY TABLESPACE temp_ts
QUOTA UNLIMITED ON my_tablespace
PROFILE default_profile
PASSWORD EXPIRE
ACCOUNT UNLOCK;

-- 3. CREATE PROFILE
CREATE PROFILE default_profile LIMIT
SESSIONS_PER_USER 3
CPU_PER_SESSION 10000
CPU_PER_CALL 1000
CONNECT_TIME 30
IDLE_TIME 10
FAILED_LOGIN_ATTEMPTS 3
PASSWORD_LIFE_TIME 60
PASSWORD_LOCK_TIME 1
PASSWORD_GRACE_TIME 7
PASSWORD_VERIFY_FUNCTION NULL;

-- 4. AUDIT syntax template
AUDIT SELECT, INSERT, UPDATE, DELETE
ON schema_name.object_name
BY username
BY SESSION
WHENEVER SUCCESSFUL;

-- End of Script 🎯
```
