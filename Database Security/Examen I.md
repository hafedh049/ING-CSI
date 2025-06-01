
![[Pasted image 20250531144654.png]]
![[Pasted image 20250601104322.png]]
![[Pasted image 20250531144704.png]]


---

Below are the corrections for the provided exam questions based on standard SQL and Oracle database practices. Since the document is in French and relates to database management (likely Oracle SQL), the answers are provided accordingly.

### Page 1
1. **Écrire la commande permettant de créer l'espace de stockage « TBL_TEK_UP » et le rendre le tablespace par défaut de la BD. (1.5 pts)**
   ```sql
   CREATE TABLESPACE TBL_TEK_UP
   DATAFILE 'TBL_TEK_UP01.dbf' SIZE 30M
   AUTOEXTEND ON NEXT 5M MAXSIZE 50M,
   DATAFILE 'TBL_TEK_UP02.dbf' SIZE 50M;
   
   ALTER DATABASE DEFAULT TABLESPACE TBL_TEK_UP;
   ```

2. **Créez un profil « PROFIL_TEK_UP » ayant les limites suivantes : (2.5 pts)**
   - Le temps d'exécution est limité à 8 heures,
   - Le temps d'inactivité est limité à 15 min,
   - Après 2 tentatives d'authentification, le compte est verrouillé pendant 5 min,
   - Le temps pendant lequel le mot de passe ne peut pas être réutilisé est 48 heures,
   - Le mot de passe contient uniquement des caractères en majuscules et des chiffres.
   ```sql
   CREATE PROFILE PROFIL_TEK_UP LIMIT
    CONNECT_TIME             480            -- 8 hours
    IDLE_TIME                15             -- 15 minutes
    FAILED_LOGIN_ATTEMPTS    2
    PASSWORD_LOCK_TIME       5/1440         -- 5 minutes
    PASSWORD_REUSE_TIME      2              -- 48 hours
    PASSWORD_VERIFY_FUNCTION VERIFY_UPPER_NUM;
   ```

3. **Créez un utilisateur « Tek_Up_Ingenieur » avec un mot de passe expiré « ING_01 », ayant le tablespace « TBL_TEK_UP » avec un quota illimité, et le profil « PROFIL_TEK_UP ». (1 pt)**
   ```sql
   CREATE USER Tek_Up_Ingenieur
   IDENTIFIED BY ING_01
   PROFILE PROFIL_TEK_UP
   DEFAULT TABLESPACE TBL_TEK_UP
   QUOTA UNLIMITED ON TBL_TEK_UP
   PASSWORD EXPIRE;
   ```

4. **Créez le rôle « ROLE_TEK_UP » ayant les privilèges suivants : (2 pts)**
   - Se connecter au serveur et créer des tables,
   - Créer des packages avec la possibilité de les déléguer,
   - Créer un index dans n'importe quel schéma de la BD,
   - Effectuer l'import et l'export de données.
   - Mettre a jour la colonne "job_id" de la table "EMPLOYEES" du schéma "HR"
   - Interroger, inserer dans la table "JOB HISTORY" du schéma "HR"
   
```SQL
CREATE ROLE ROLE_TEK_UP;

GRANT 
  CREATE SESSION, 
  CREATE TABLE, 
  CREATE PACKAGE, 
  CREATE PACKAGE BODY 
    TO ROLE_TEK_UP WITH ADMIN OPTION;

GRANT 
  CREATE ANY INDEX, 
  IMP_FULL_DATABASE, 
  EXP_FULL_DATABASE 
    TO ROLE_TEK_UP;

GRANT 
  UPDATE (job_id) ON HR.EMPLOYEES, 
  SELECT, INSERT ON HR.JOB_HISTORY 
    TO ROLE_TEK_UP;
   ```

5. **Affecter le rôle « ROLE_TEK_UP » à l'utilisateur « TEK_UP_Ingenieur ». (0.5 pt)**
   ```sql
   GRANT ROLE_TEK_UP TO Tek_Up_Ingenieur;
   ```

### Page 2
6. **Créez une procédure stockée « EXPIRE_PW_USER » qui permet de faire expirer le mot de passe d'un utilisateur dont le nom est passé en argument. (2 pts)**
   ```sql
   CREATE OR REPLACE PROCEDURE EXPIRE_PW_USER (p_username IN VARCHAR2) AS
   BEGIN
       EXECUTE IMMEDIATE 'ALTER USER ' || p_username || ' PASSWORD EXPIRE';
   END EXPIRE_PW_USER;
   /
   ```

7. **Créez une procédure stockée « EXPIRE_PW_ALL_USERS » qui permet de faire expirer le mot de passe de tous les utilisateurs. NB: utiliser la première procédure. (2 pts)**
   ```sql
   CREATE OR REPLACE PROCEDURE EXPIRE_PW_ALL_USERS AS / IS
   BEGIN
       FOR user_rec IN (SELECT username FROM dba_users WHERE account_status = 'OPEN')
       LOOP
           EXPIRE_PW_USER(user_rec.username);
       END LOOP;
   END EXPIRE_PW_ALL_USERS;
   /
   ```

8. **Afin de surveiller les actions exécutées sur la base de données (2 pts):**
   - Visualisez le paramètre d'audit et l'activer (utiliser l'option DB).
   - Lancez l'audit pour l'utilisateur « Tek_Up_Ingenieur » afin de surveiller toutes les instructions DDL et DML (Par session).
   - Lancez l'audit afin de surveiller toutes les instructions insert, update et delete sur la table Etudiants. (Par accès).
   - Listez toutes opérations effectuées par « Tek_Up_Ingenieur » sur des objets. (La date de l'action, le nom de l'objet ainsi que le nom de l'action).
   - Enlevez l'audit d'insert sur la table Etudiants.
   ```sql
   -- Visualiser et activer l'audit
   SHOW PARAMETER audit_trail;
   ALTER SYSTEM SET audit_trail = DB SCOPE = SPFILE;
   
   -- Audit pour l'utilisateur
   AUDIT ALL BY Tek_Up_Ingenieur BY SESSION;
   
   -- Audit sur la table Etudiants
   AUDIT INSERT, UPDATE, DELETE ON Etudiants BY ACCESS;
   
   -- Lister les opérations
   SELECT username, obj_name, action_name, timestamp
   FROM dba_audit_trail
   WHERE username = 'TEK_UP_INGENIEUR';
   
   -- Enlever l'audit d'insert
   NOAUDIT INSERT ON Etudiants;
   ```

9. **Créez une procédure stockée « PROC_AUDIT » qui affiche les traces d'audit de création d'utilisateurs. On souhaite afficher l'utilisateur qui a lancé la commande et la date de lancement. (2 pts)**
   ```sql
   CREATE OR REPLACE PROCEDURE PROC_AUDIT AS
   BEGIN
       FOR audit_rec IN (SELECT username, action_name, timestamp
                        FROM dba_audit_trail
                        WHERE action_name = 'CREATE USER')
       LOOP
           DBMS_OUTPUT.PUT_LINE('Utilisateur: ' || audit_rec.username || 
                              ', Date: ' || audit_rec.timestamp);
       END LOOP;
   END PROC_AUDIT;
   /
   ```

10. **On souhaite conserver des statistiques concernant les mises à jour (insertions, modifications, suppressions) sur la table Etudiants.**
    - A. Créez à l'aide de SQL la structure de la table STATS et la remplir comme suit. (0.5 pt)
    ```sql
    CREATE TABLE STATS (
        TypeMaj VARCHAR2(10),
        NbMaj NUMBER,
        Date_derniere_Maj DATE
    );
    
    INSERT INTO STATS VALUES ('INSERT', 0, NULL);
    INSERT INTO STATS VALUES ('UPDATE', 0, NULL);
    INSERT INTO STATS VALUES ('DELETE', 0, NULL);
    ```

    - B. Définir un déclencheur après insertion/modification/suppression dans la table Etudiants permettant de mettre à jour automatiquement la table STATS. (2 pts)
    ```sql
    CREATE OR REPLACE TRIGGER trg_update_stats
    AFTER INSERT OR UPDATE OR DELETE ON Etudiants
    FOR EACH ROW
    DECLARE
        v_type VARCHAR2(10);
    BEGIN
        IF INSERTING THEN v_type := 'INSERT';
        ELSIF UPDATING THEN v_type := 'UPDATE';
        ELSIF DELETING THEN v_type := 'DELETE';
        END IF;
        
        UPDATE STATS
        SET NbMaj = NbMaj + 1,
            Date_derniere_Maj = SYSDATE
        WHERE TypeMaj = v_type;
    END trg_update_stats;
    /

	-----
	SELECT SYSDATE FROM DUAL;
	```

11. **Écrire une requête permettant de récupérer les lignes supprimées. (Utiliser AS OF SCN) (1 pt)**
    ```sql
    SELECT * FROM Etudiants AS OF SCN 200331;
    ```

12. **La table Etudiants vient d'être supprimée. Donner la commande permettant de la restaurer. (1 pt)**
    ```sql
    FLASHBACK TABLE Etudiants TO BEFORE DROP;
    ```