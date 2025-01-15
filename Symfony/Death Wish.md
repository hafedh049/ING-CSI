# I - QCM ($6 \text{pts}$)

![[Pasted image 20250115112700.png]]

![[Pasted image 20250115112902.png]]

![[Pasted image 20250115113257.png]]

![[Pasted image 20250115113356.png]]

![[Pasted image 20250115113534.png]]

![[Pasted image 20250115113906.png]]

# II - Partie 2 ($14 \text{pts}$)

![[Pasted image 20250115114050.png]]

```php
symfony new --webapp ExamenSSIR
composer create-project symfony/skeleton:"7.2.*" ExamenSSIR && composer require webapp
```

![[Pasted image 20250115114915.png]]

```yaml
#DATABASE_URL="sqlite:///kernel.project_dirk/var/data.db"
DATABASE_URL="mysql://root:@localhost:3306/mydb&serverversion-8&charset=utf8mb4"
#DATABASE_URL"postgresql://app:ChangeMel@127.0.0.1:5432/app/serverVersion=15&charset=utf8"
```

![[Pasted image 20250115115255.png]]

```css
symfony console doctrine:database:create
#OR
symfony console d:d:c
#OR
php bin/console doctrine:database:create
#OR
symfony console d:d:c
```

![[Pasted image 20250115120619.png]]
### Step 1: **Create User Entity**

- **Command**: `php bin/console make:user`
- **Files created**:
    - `src/Entity/User.php` (2)
    - `src/Repository/UserRepository.php` (1)

### Step 2: **Generate the Authenticator**

- **Command**: `php bin/console make:auth`
- **Files created**:
    - `src/Security/LoginFormAuthenticator.php` (2)

### Step 3: **Create Registration Form**

- **Command**: `php bin/console make:registration-form`
- **Files created**:
    - `src/Controller/RegistrationController.php` (3)
    - `templates/registration/register.html.twig` (1)
    - `src/Form/RegistrationFormType.php` (3)

### Step 4: **Generate Password Reset Functionality**

- **Command**: `php bin/console make:reset-password`
- **Files created**:
    - `src/Controller/PasswordResetController.php` (3)
    - `templates/security/reset_password.html.twig` (1)
    - `templates/security/forgot_password.html.twig` (3)

![[Pasted image 20250115120802.png]]
### Step-by-Step Breakdown:

1. **Variable Initialization:**
    
    ```php
    var $uploadedPhotoFile = $form->get('photo')->getData();
    ```
    
    - **Purpose:** This line gets the data of the file uploaded via a form field named `photo`. The `$form->get('photo')->getData()` retrieves the uploaded file and stores it in the `$uploadedPhotoFile` variable.
    - **Explanation:** In Symfony, when dealing with file uploads in forms, `getData()` will return the file object (e.g., an instance of `UploadedFile`).
2. **Extracting File Information:**
    
    ```php
    $pathinfo = pathinfo($uploadedPhotoFile->getClientOriginalName(), PATHINFO_FILENAME);
    ```
    
    - **Purpose:** This line gets the original filename of the uploaded file (without the extension) using `getClientOriginalName()`.
    - **Explanation:** `pathinfo()` is a PHP function that breaks down a file path into various components (e.g., name, extension). Here, `PATHINFO_FILENAME` retrieves the filename without the extension.
3. **Checking if File Exists and Assigning Filename:**
    
    ```php
    if ($uploadedPhotoFile) {
        $filename = $uploadedPhotoFile->getClientOriginalName();
    }
    ```
    
    - **Purpose:** This checks if a file has actually been uploaded and assigns the original filename to `$filename`.
    - **Explanation:** The `if ($uploadedPhotoFile)` ensures that the code only proceeds with file handling if a file was uploaded. The `getClientOriginalName()` method returns the original name of the file, including its extension.
4. **Try-Catch Block for File Handling:**
    
    ```php
    try {
        $slugger = new \Symfony\Component\String\Slugger\AsciiSlugger();
        $newFilename = $slugger->slug($pathinfo)->toString();
        $newFilename .= '-' . uniqid() . '.' . $uploadedPhotoFile->guessExtension();
        $uploadedPhotoFile->move(
            $this->getParameter('photos_directory'),
            $newFilename
        );
    } catch (FileException $e) {
        // Handle exception
    }
    ```
    
    - **Purpose:** This block handles generating a safe, unique filename for the uploaded file and then moves it to a specific directory.
    - **Explanation:**
        - **Slugger:** A `Slugger` object is created, which is used to convert the filename to a URL-safe format (i.e., converting spaces to hyphens and ensuring it's ASCII).
        - **New Filename:** The `slug()` method is applied to the original filename (without extension) to create a "slugged" version. Then, a unique ID is appended to ensure the filename is unique. The file extension is obtained using `guessExtension()`.
        - **Move File:** The file is moved to a directory defined by the `photos_directory` parameter in Symfony’s configuration (`getParameter('photos_directory')`). The file is saved with the newly generated filename.
        - **Exception Handling:** If there is an error during file upload or movement, a `FileException` will be thrown, and the exception can be handled in the `catch` block.
5. **Setting the Photo for the Member:**
    
    ```php
    $member->setPhoto($newFilename);
    ```
    
    - **Purpose:** This line associates the uploaded file (now with a unique name) with the `member` entity.
    - **Explanation:** After the file has been uploaded and stored, the `$member` object’s `setPhoto()` method is called to store the new filename (e.g., in the database) for later use (e.g., to display the photo on the member's profile).

### What This Code Does:

- The code handles file upload for a photo field in a form.
- It extracts the original file name and generates a new unique, sanitized filename using a slugger and a unique ID.
- The file is then moved to a designated directory (set in the Symfony configuration).
- After the file is successfully uploaded, the `member` entity is updated with the new filename.
- If an error occurs during the upload, it is caught by the `try-catch` block, and no error message is displayed directly (though you can handle it as needed).

### Why This Is Useful:

- **Sanitizing Filenames:** Using a slugger ensures that filenames are safe for use in URLs, preventing issues with spaces or special characters.
- **Unique Filenames:** The addition of `uniqid()` guarantees that no two files will have the same name, even if multiple users upload the same file.
- **Error Handling:** The `try-catch` block helps prevent the application from crashing if an error occurs during the upload process.

![[Pasted image 20250115121345.png]]

```php
src\Controller\
```

![[Pasted image 20250115121436.png]]

```php
symfony console make:form
# OR
php bin/console make:form
```

```php
$builder->add('photo', FileType::class, ['data_class' => null])
```