1. Donner la liste des vues du dictionnaire de données d'Oracle triée par nom.
```SQL
select table_name 
from dict
order by table_name;
```

2. Donner la liste des utilisateurs créés sur le serveur. Afficher leur nom et la date de leur création.
```SQL
select username 
from dba_users;
```

3. Donner la liste des utilisateurs connectés sur votre instance courante.
```SQL
select username 
from v$session 
where username is not null;
```

4. Déterminer la taille totale de la SGA.
```SQL
select sum(value) / 1024 / 1024 AS SGA_SIZE_MB 
from v$sga;
```

- Connectez-vous avec le compte HR
```SQL
connect sys/as SYSDBA;
alter user hr account unlock;
alter user hr indetified by hr;
connect hr/hr
```

5. Affichez la liste de ses objets, leur type, la date de création et la date de dernière modification
```SQL
select object_name, object_type, created, last_ddl_time
from user_objects;
```

6. Afficher les noms des tables sur lesquelles il a des droits.
```SQL
select table_name from all_tables
where owner='HR';
```

7. Ecrire une procédure stockée qui permet d’afficher les noms de ses tables propriétaires.
```SQL
SET SERVEROUTPUT ON;

CREATE OR REPLACE PROCEDURE afficher_tables_proprietaire() 
IS
BEGIN
    FOR rec IN (SELECT table_name FROM user_tables) LOOP
        DBMS_OUTPUT.PUT_LINE(rec.table_name);
    END LOOP;
END;
/

EXEC afficher_tables_proprietaire;
```

- Connectez-vous avec le compte Administrateur
```SQL
connect sys/as SYSDBA;
```

8. Afficher le nombre total des tables créés dans le serveur.
```SQL
select count(*) as TOTAL_TABLES
from dba_tables;
```

9. Afficher le nombre total de table créés par l’utilisateur HR.
```SQL
select count(*) as TOTAL_TABLES
from dba_tables
where owner = 'HR';
```

10. Ecrire une fonction stockée qui prend en paramètre un utilisateur et retourne le nombre de ses objets.
```SQL
CREATE OR REPLACE FUNCTION get_user_object_count(p_username VARCHAR2) 
RETURN NUMBER IS 
    v_count NUMBER;
BEGIN
    SELECT COUNT(*) INTO v_count 
    FROM all_objects 
    WHERE owner = UPPER(p_username);
    RETURN v_count;
END;
/

SELECT get_user_object_count('HR') FROM dual;
```

11. Créer une procédure stockée qui permet de lister les tables relatives à l’utilisateur donné en paramètre. Tester la procédure avec l’utilisateur HR.
```SQL
SET SERVEROUTPUT ON;

CREATE OR REPLACE PROCEDURE list_tables(user IN VARCHAR2) 
IS
BEGIN
    FOR rec IN (SELECT table_name FROM all_tables where owner=UPPER(user)) LOOP
        DBMS_OUTPUT.PUT_LINE(rec.table_name);
    END LOOP;
END;
/

EXEC list_tables('HR');
```