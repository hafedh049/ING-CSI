### **1. General Commands**

- **Check Symfony Version**
    
    ```c
    php bin/console --version
    ```
    
- **List All Commands**
    
    ```c
    php bin/console list
    ```
    

---

### **2. Cache Management**

- **Clear Cache**
    
    ```c
    php bin/console cache:clear
    ```
    
- **Warmup Cache**
    
    ```c
    php bin/console cache:warmup
    ```
    

---

### **3. Environment Management**

- **Check Current Environment**
    
    ```c
    php bin/console debug:container --env-vars
    ```
    
- **Run Commands in a Specific Environment**
    
    ```c
    php bin/console <command_name> --env=prod
    ```
    

---

### **4. Doctrine (Database)**

- **Create Database**
    
    ```c
    php bin/console doctrine:database:create
    ```
    
- **Drop Database**
    
    ```c
    php bin/console doctrine:database:drop --force
    ```
    
- **Run Migrations**
    
    ```c
    php bin/console doctrine:migrations:migrate
    ```
    
- **Generate Entity Getter/Setter**
    
    ```c
    php bin/console make:entity
    ```
    

---

### **5. Debugging**

- **View Routes**
    
    ```c
    php bin/console debug:router
    ```
    
- **View Configurations**
    
    ```c
    php bin/console debug:config <service_name>
    ```
    
- **View Container Services**
    
    ```c
    php bin/console debug:container
    ```
    

---

### **6. Make Commands (Code Generation)**

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
    
- **Create a Command**
    
    ```c
    php bin/console make:command ExampleCommand
    ```
    
- **Create a Service**
    
    ```c
    php bin/console make:service ExampleService
    ```
    

---

### **7. Security**

- **Generate User Security Entity**
    
    ```c
    php bin/console make:user
    ```
    
- **Check Security Configurations**
    
    ```c
    php bin/console security:check
    ```
    

---

### **8. Assets and Translations**

- **Install Assets**
    
    ```c
    php bin/console assets:install public
    ```
    
- **Update Translations**
    
    ```c
    php bin/console translation:update --force en
    ```
    

---

### **9. Server Management**

- **Run Built-in Server**
    
    ```c
    php bin/console server:start
    ```
    
- **Stop Built-in Server**
    
    ```c
    php bin/console server:stop
    ```
    

---

### **10. Custom Commands**

- **Execute Custom Command**  
    If you have created a custom command, execute it as follows:
    
    ```c
    php bin/console app:custom-command
    ```
    

---

### **11. Workflow**

- **Dump Workflow**
    
    ```c
    php bin/console workflow:dump <workflow_name> | dot -Tpng -o workflow.png
    ```
    

---

### **12. Event Dispatcher**

- **Debug Events and Listeners**
    
    ```c
    php bin/console debug:event-dispatcher
    ```
    

---

These commands serve as a starting point for managing and developing your Symfony project effectively. Each command often has additional options; use the `--help` flag for details, e.g., `php bin/console cache:clear --help`.