## **1. Procedure Structure**

A **procedure** is a subprogram that performs an action but does **not** return a value.
### **Syntax**

```sql
CREATE [OR REPLACE] PROCEDURE procedure_name (
    parameter1 [IN | OUT | IN OUT] datatype,
    parameter2 [IN | OUT | IN OUT] datatype
) 
IS
    -- Declarations (variables, constants, cursors)
BEGIN
    -- Executable statements
    -- Business logic implementation
EXCEPTION
    -- Exception handling
END procedure_name;
```

### **Example**

```sql
CREATE OR REPLACE PROCEDURE update_salary (
    emp_id IN NUMBER,
    new_salary IN NUMBER
) 
IS
BEGIN
    UPDATE employees SET salary = new_salary WHERE employee_id = emp_id;
    COMMIT;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error updating salary: ' || SQLERRM);
END update_salary;
```

---

## **2. Function Structure**

A **function** is a subprogram that returns a single value.

### **Syntax**

```sql
CREATE [OR REPLACE] FUNCTION function_name (
    parameter1 [IN] datatype,
    parameter2 [IN] datatype
) RETURN return_datatype 
IS
    -- Declarations (variables, constants, cursors)
BEGIN
    -- Executable statements
    RETURN some_value;
EXCEPTION
    -- Exception handling
END function_name;
```

### **Example**

```sql
CREATE OR REPLACE FUNCTION get_employee_salary (
    emp_id IN NUMBER
) RETURN NUMBER 
IS
    emp_salary NUMBER;
BEGIN
    SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
    RETURN emp_salary;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 0;
    WHEN OTHERS THEN
        RETURN -1;
END get_employee_salary;
```

---

## **Key Differences**

|Feature|Procedure|Function|
|---|---|---|
|Return Value|No return value (only `OUT` parameters)|Must return a single value|
|Usage|Used for actions like updates, inserts, deletes|Used for calculations or retrieving values|
|Called In SQL?|No|Yes (if no DML statements inside)|

---

In PL/SQL, the `/` at the end of a **procedure** or **function** is used to execute the entire block in SQL*Plus or other tools that process PL/SQL scripts. It is not part of the PL/SQL syntax itself but rather a **command separator** for the client tool.

---

### **Why is `/` needed?**

1. **PL/SQL Blocks Are Not Automatically Executed**  
    Unlike `SELECT`, `INSERT`, or `UPDATE` statements, a PL/SQL block (like a `CREATE PROCEDURE` or `CREATE FUNCTION`) is treated as a multi-line command. SQL*Plus (or tools like SQLcl, Toad, or SQL Developer) need `/` to know when the block ends.
    
2. **Separates the PL/SQL Block from the Next Command**  
    The `/` tells SQL_Plus that the preceding block should be sent to the database for execution. Without it, SQL_Plus may just store the block in memory without executing it.
    

---

### **Example Without `/` (in SQL*Plus)**

```sql
CREATE OR REPLACE PROCEDURE test_proc IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
END test_proc;
```

🔴 In SQL*Plus, this won't execute immediately.

---

### **Example With `/` (Correct)**

```sql
CREATE OR REPLACE PROCEDURE test_proc IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
END test_proc;
/
```

✅ The `/` executes the procedure creation.

---

### **When is `/` Not Needed?**

- If you're running PL/SQL code inside a PL/SQL anonymous block:
    
    ```sql
    BEGIN
        DBMS_OUTPUT.PUT_LINE('Hello!');
    END;
    ```
    
    No `/` is needed here because the `;` terminates the block.
    
- If you're executing a procedure in SQL Developer (not SQL*Plus), clicking **Run** will execute the block automatically.
    

---

### **Alternative in SQL Developer**

In SQL Developer or tools using **PL/SQL engine**, you can also use:

```sql
SHOW ERRORS;
```

after running the procedure to check for any issues.

---

### **Summary**

✅ The `/` is a **command separator** in SQL*Plus, SQLcl, and some other tools.  
✅ It signals the end of a **procedure** or **function** definition.  
✅ It's **not part of PL/SQL syntax** but needed for execution in some environments.

