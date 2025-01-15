### **1. SQL Injection**

#### **Attack Examples**

1. **Authentication Bypass**:
    
    ```sql
    SELECT * FROM users WHERE username = 'admin' AND password = '' OR '1'='1';
    ```
    
    This always evaluates to `TRUE`, bypassing authentication.
    
2. **Retrieving Hidden Data**:
    
    ```sql
    SELECT * FROM products WHERE category = 'electronics' OR '1'='1';
    ```
    
    This reveals all products regardless of the category.
    
3. **Modifying Data**:
    
    ```sql
    UPDATE users SET password = 'hacked' WHERE username = 'admin'; DROP TABLE users;
    ```
    
    Injected payload modifies a password and deletes the `users` table.
    

---

#### **Symfony's Solutions**

1. **Parameterized Queries**:
    
    ```php
    $query = $entityManager->createQuery(
        'SELECT u FROM App\Entity\User u WHERE u.username = :username'
    )->setParameter('username', $username);
    ```
    
    Parameters are automatically sanitized.
    
2. **Query Builder**:
    
    ```php
    $queryBuilder = $entityManager->createQueryBuilder();
    $queryBuilder->select('u')
        ->from('App\Entity\User', 'u')
        ->where('u.username = :username')
        ->setParameter('username', $username);
    ```
    

---

### **2. Cross-Site Scripting (XSS)**

#### **Attack Examples**

1. **Stored XSS**:
    
    - An attacker submits malicious input stored in the database and displayed to other users.
    
    ```html
    <script>alert('Your account has been hacked!');</script>
    ```
    
2. **Reflected XSS**:
    
    - Malicious scripts are included in the URL or request parameters.
    
    ```html
    <a href="http://example.com?name=<script>alert('Hacked!');</script>">Click me</a>
    ```
    
3. **DOM-Based XSS**:
    
    - Scripts injected into the client-side dynamically modify the DOM.
    
    ```javascript
    document.write('<img src="malicious-image.jpg" />');
    ```
    

---

#### **Symfony's Solutions**

1. **Twig Escaping**:
    
    - By default, Twig escapes all variables.
    
    ```twig
    {{ user.name }} <!-- Converts `<script>` to `&lt;script&gt;` -->
    ```
    
2. **Content Security Policy (CSP)**:
    
    - Restricts allowed sources of scripts.
    
    ```php
    $response->headers->set('Content-Security-Policy', "script-src 'self'");
    ```
    

---

### **3. Cross-Site Request Forgery (CSRF)**

#### **Attack Examples**

1. **Form Submission**:
    
    - An attacker tricks the user into submitting a malicious form.
    
    ```html
    <form action="http://example.com/change-password" method="POST">
        <input type="hidden" name="password" value="new-password">
        <button type="submit">Click me!</button>
    </form>
    ```
    
2. **Image Tag Exploit**:
    
    ```html
    <img src="http://example.com/transfer-funds?amount=1000&to=attacker" />
    ```
    

---

#### **Symfony's Solutions**

1. **CSRF Token**:
    
    - Symfony provides built-in CSRF protection.
    
    ```php
    $form = $this->createFormBuilder()
        ->add('csrf', HiddenType::class, [
            'data' => $csrfTokenManager->getToken('unique_token_id')->getValue(),
        ])
        ->getForm();
    ```
    
2. **Validation in Forms**:
    
    - Symfony forms automatically validate CSRF tokens if enabled.

---

### **4. Session Hijacking**

#### **Attack Examples**

1. **Session ID Guessing**:
    
    - An attacker guesses or predicts session IDs.
2. **Session Fixation**:
    
    - An attacker sets a session ID and forces the victim to use it.

---

#### **Symfony's Solutions**

1. **Secure Cookies**:
    
    - Symfony sets cookies with secure attributes:
    
    ```yaml
    framework:
        session:
            cookie_secure: auto
            cookie_httponly: true
    ```
    
2. **Regenerate Session IDs**:
    
    ```php
    $request->getSession()->migrate();
    ```
    

---

### **5. Directory Traversal**

#### **Attack Examples**

1. **Access Restricted Files**:
    
    ```bash
    ../../etc/passwd
    ```
    
2. **Access Source Code**:
    
    ```bash
    ../../app/config/config.yaml
    ```
    

---

#### **Symfony's Solutions**

1. **Strict File Paths**:
    
    - Symfony validates file paths using secure helpers.
    
    ```php
    $filePath = realpath($directory . '/' . $filename);
    if (strpos($filePath, $directory) !== 0) {
        throw new \Exception('Invalid file path!');
    }
    ```
    
2. **Disallow Unsafe Input**:
    
    - Use Symfony components like `Symfony\Component\Filesystem` for secure file handling.

---

### **6. Security Misconfiguration**

#### **Attack Examples**

1. **Default Credentials**:
    
    - Using default admin credentials (e.g., `admin:admin`).
2. **Exposed Debug Mode**:
    
    - Exposing sensitive information via debug mode.

---

#### **Symfony's Solutions**

1. **Environment Variables**:
    
    - Symfony uses `.env` files to store sensitive configurations securely.
2. **Disable Debug in Production**:
    
    ```php
    // Set APP_ENV=prod in .env file
    ```
    

---

### **7. File Upload Vulnerabilities**

#### **Attack Examples**

1. **Malicious File Upload**:
    
    - An attacker uploads a script disguised as an image.
    
    ```php
    <script>alert('Hacked');</script>
    ```
    

---

#### **Symfony's Solutions**

1. **File Validation**:
    
    - Symfony validates file extensions and MIME types.
    
    ```php
    $form->add('file', FileType::class, [
        'constraints' => [
            new File([
                'maxSize' => '1024k',
                'mimeTypes' => [
                    'image/jpeg',
                    'image/png',
                ],
            ]),
        ],
    ]);
    ```
    
2. **Upload Directory Restrictions**:
    
    - Symfony isolates uploads into secure directories.

---

### **Summary Table**

|**Attack**|**Examples**|**Symfony's Solution**|
|---|---|---|
|**SQL Injection**|Auth bypass, data leaks, schema destruction|Doctrine ORM, query builder, prepared statements|
|**XSS**|Stored, reflected, DOM-based XSS|Twig escaping, CSP|
|**CSRF**|Hidden forms, image tag exploit|CSRF tokens in forms|
|**Session Hijacking**|ID guessing, fixation|Secure cookies, session regeneration|
|**Directory Traversal**|Accessing `/etc/passwd` or configs|Path validation, secure file helpers|
|**Security Misconfiguration**|Default creds, exposed debug mode|Environment variables, disable debug|
|**File Upload**|Uploading malicious scripts|File validation, upload directory restrictions|

Symfony provides an extensive and secure foundation to protect against these vulnerabilities, making it a powerful choice for secure web applications.