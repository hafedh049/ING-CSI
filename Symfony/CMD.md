### **1. General Commands**

- **Create Symfony Project**
    
	```c
	// The '--webapp' option indicated that it is a full web app with frontend and backend support
	symfony new --webapp MyApp
	## OR
	composer create-project symfony/skeleton:"7.2.*" MyApp 
	```
	
- **Check Symfony Version**
    
    ```c
    php bin/console --version
    symfony console --version
    ```
    
- **List All Commands**
    
    ```c
    php bin/console list
    symfony console list
    ```
    

---

### **2. Cache Management**

- **Clear Cache**
    
    ```c
    php bin/console cache:clear
    symfony console cache:clear
    ```
    

---
### **3. Doctrine (Database)** (ORM)

- **Create Database**
    
    ```c
    php bin/console doctrine:database:create
    (.env) // The config file
    ```
    
- **Drop Database**
    
    ```c
    php bin/console doctrine:database:drop --force
    ```
    
- **Run Migrations**
    
    ```c
    // Generate the Versions files AKA the sql files
    symfony console make:migration 
    // Execute it against the DB (MYSQL)
    php bin/console doctrine:migrations:migrate
    ```
    
- **Generate Entity Getter/Setter**
    
    ```c
    php bin/console make:entity
    ```
    

---
### **4. Make Commands (Code Generation)**

- **Create CRUD** (ALL)
    
    ```c
    php bin/console make:crud entity_name
    ```
    
- **Create a Controller**
    
    ```c
    php bin/console make:controller ExampleController
    ```
    
- **Create a Form**
    
    ```c
    php bin/console make:form ExampleType
    ```
    

---

### **5. Security**

- **Generate User Security Entity**
    
    ```c
    php bin/console make:user
    ```
    
- **Make Registration Form**
    
    ```c
    php bin/console make:registration-form
    ```
    
- **Create Authentification**
    
    ```c
    php bin/console make:auth
    ```
    


---

### **6. Server Management**

- **Run Built-in Server**
    
    ```c
    php bin/console server:start
    symfony server:start
    ```
    
- **Stop Built-in Server**
    
    ```c
    php bin/console server:stop
    ```
    