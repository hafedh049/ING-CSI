# Partie I

## 1. Donner les composants de l'insatance du serveur ORACLE

> [!Tip]
> **SGA**
> **PGA**
> **Background Processes**

## 2. Donner les étapes pour faire passer la base de données du mode NO-ARCHIVE-LOG vers ARCHIVE-LOG

1. Vérifier le mode actuel :
    
    ```sql
    SELECT log_mode FROM v$database;
    ```
    
2. Arrêter et redémarrer la base en mode MOUNT :
    
    ```sql
    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ```
    
3. Activer le mode ARCHIVELOG :
    
    ```sql
    ALTER DATABASE ARCHIVELOG;
    ```
    
4. Ouvrir la base de données :
    
    ```sql
    ALTER DATABASE OPEN;
    ```

# Partie II

1. Créer un tablespace **TBL_APP_COM** de Taille 20 Mo reparti en 2 fichiers:
	- **C:\oraclexe\oradata\xe\fd01tbl_app.dbf** de taille **10Mo** extensible de **2M**
	- **C:\oraclexe\oradata\xe\fd02tbl_app.dbf** de taille fixe de **10Mo**

```SQL
CREATE TABLESPACE TBL_APP_COM 
DATAFILE 'C:\oraclexe\oradata\xe\fd01tbl_app.dbf'
SIZE 10M
AUTOEXTEND ON
NEXT 2M,
DATAFILE 'C:\oraclexe\oradata\xe\fd02tbl_app.dbf'
SIZE 10M;
```

2. Créer un tablespace temporaire **TEMP_APP_COM** de taille **20M** contenant un fichier:
	- **C:\oraclexe\oradata\XE\ftemp_app.dbf**

```SQL
CREATE TEMPORARY TABLESPACE TEMP_APP_COM 
DATAFILE 'C:\oraclexe\oradata\xe\ftemp_app.dbf'
SIZE 20M;
```

3. Créer un utilisateur de nom **A_C_MANAGER** avec :
	- Un mot de passe expiré de valeur **a_c_manager**
	- **TBL_APP_COM** comme tablespace par défaut
	- **TEMP_APP_COM** comme tablespace temporaire

```SQL
CREATE USER A_C_MANAGER IDENTIFIED a_c_manager
DEFAULT TABLESPACE TBL_APP_COM
TEMPORARY TABLESPACE TEMP_APP_COM;
```

4. Accorder à l'utilisateur **A_C_MANAGER**:
	- Les priviléges de connexion à la base de données avec la possibilité de le transmettre
	- Les priviléges de lecture et d'insertion sur la table **EMPLOYEES** de l'utilisateur **HR**
	- Le role **RESOURCE**

```SQL
GRANT CREATE SESSION TO A_C_MANAGER WITH ADMIN OPTION;
GRANT SELECT, INSERT ON HR.EMPLOYEES TO A_C_MANAGER;
GRANT RESOURCE TO A_C_MANAGER;
```

5. Modifier les proprietes de la base de données afin de rendre les tablespaces **TBL_APP_COM** et **TEMP_APP** comme tablespace par defaut

```SQL
ALTER DATABASE DEFAULT TABLESPACE TBL_APP_COM;  
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE TEMP_APP;
```

# Partie III

1. A
2. B
3. B
4. D
5. C
6. C
7. C
8. B
9. A, C
10. B