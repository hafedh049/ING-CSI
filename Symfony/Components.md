### **I. Controller**

A **Controller** is a PHP class that defines methods (called "actions") to handle incoming HTTP requests. It serves as the glue between the user's request, the application logic, and the response.

#### Responsibilities:

- Handle HTTP requests.
- Interact with services, repositories, or other components to process the request.
- Return an HTTP response (e.g., HTML, JSON, or XML).

#### Example:

```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class ExampleController extends AbstractController
{
    public function index(): Response
    {
        return $this->render('example/index.html.twig', [
            'message' => 'Hello, Symfony!',
        ]);
    }
}
```


In Symfony, controllers are responsible for handling requests and returning responses. Below is a detailed overview of the common controller syntax, methods, and best practices:

### Basic Controller Syntax

A controller is usually a PHP class with methods that handle different routes. Here’s the general structure:

```php
// src/Controller/YourController.php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class YourController extends AbstractController
{
    /**
     * @Route("/route", name="your_route_name")
     */
    public function yourAction(): Response
    {
        return new Response('Hello World');
    }
}
```

### 1. **Annotations for Routing**

Symfony allows you to define routes using annotations (`@Route`) or YAML/XML configurations.

- **Route Annotation**:
    
    ```php
    /**
     * @Route("/home", name="home")
     */
    public function home()
    {
        return $this->render('home.html.twig');
    }
    ```
    
- **Route with Parameters**:
    
    ```php
    /**
     * @Route("/article/{id}", name="article_show")
     */
    public function show($id)
    {
        // Use the $id parameter
        return $this->render('article/show.html.twig', ['id' => $id]);
    }
    ```
### 2. **Basic Action Methods**

The action methods handle requests and can return different types of responses. The most common response types are `Response`, `JsonResponse`, and `RedirectResponse`.

- **Rendering a Twig Template**:
    
    ```php
    public function show()
    {
        return $this->render('page/show.html.twig', [
            'variable' => 'value',
        ]);
    }
    ```
    
- **Redirecting to Another Route**:
    
    ```php
    public function redirectToRoute()
    {
        return $this->redirectToRoute('home');
    }
    ```
    
- **Redirecting to a URL**:
    
    ```php
    public function redirectToUrl()
    {
        return $this->redirect('https://www.example.com');
    }
    ```
    

### 3. **Using Query Parameters**

You can access query parameters in controller methods through the request object:

```php
public function index(Request $request)
{
	#GET
    $searchTerm = $request->query->get('search', 'default');
    return $this->render('index.html.twig', ['search' => $searchTerm]);
}
```

### 4. **Handling POST Requests**

To handle POST requests, you can use the `Request` object to retrieve form data or JSON input.

- **Handling Form Data**:
    
    ```php
    public function submitForm(Request $request)
    {
        $name = $request->request->get('name');
        return $this->render('submit.html.twig', ['name' => $name]);
    }
    ```
    

### 5. **Using Dependency Injection**

You can inject services into the controller’s constructor or directly into methods using the `@required` annotation.

- **Constructor Injection**:
    
    ```php
    class YourController extends AbstractController
    {
        private $service;
    
        public function __construct(YourService $service)
        {
            $this->service = $service;
        }
    
        public function index()
        {
            $result = $this->service->getData();
            return $this->render('index.html.twig', ['data' => $result]);
        }
    }
    ```
    
- **Method Injection**:
    
    ```php
    public function index(YourService $service)
    {
        $result = $service->getData();
        return $this->render('index.html.twig', ['data' => $result]);
    }
    ```
    

### 6. **Accessing the Session**

You can access the session in controllers to store or retrieve session data:

```php
public function setSessionData(Request $request)
{
    $session = $request->getSession();
    $session->set('username', 'JohnDoe');
    return new Response('Session data set!');
}

public function getSessionData(Request $request)
{
    $session = $request->getSession();
    $username = $session->get('username', 'Guest');
    return $this->render('welcome.html.twig', ['username' => $username]);
}
```

### 7. **Flash Messages**

Flash messages are used to pass temporary data between requests (often for success or error messages).

- **Setting a Flash Message**:
    
    ```php
    public function addFlashMessage()
    {
        $this->addFlash('notice', 'Your changes have been saved!');
        return $this->redirectToRoute('home');
    }
    ```
    
- **Displaying Flash Messages in Twig**:
    
    ```twig
    {% for message in app.flashes('notice') %}
        <div class="alert alert-success">{{ message }}</div>
    {% endfor %}
    ```
    
### 8. **Form Handling in Controller**

You can handle forms using Symfony's Form component.

```php
use App\Form\YourFormType;
use App\Entity\YourEntity;

public function formSubmit(Request $request)
{
    $entity = new YourEntity();
    $form = $this->createForm(YourFormType::class, $entity);

    $form->handleRequest($request);

    if ($form->isSubmitted() && $form->isValid()) {
        // Handle the form submission
        return $this->redirectToRoute('success');
    }

    return $this->render('form.html.twig', [
        'form' => $form->createView(),
    ]);
}
```

### 9. **Accessing the User**

If you need to access the authenticated user, you can use the `getUser()` method of the `AbstractController`.

```php
public function dashboard()
{
    $user = $this->getUser();
    return $this->render('dashboard.html.twig', ['user' => $user]);
}
```

### 10. **Error Handling**

You can define custom error handling for your controller actions using `try-catch` blocks or error templates.

```php
public function show()
{
    try {
        // Some logic that might throw an exception
    } catch (\Exception $e) {
        return $this->render('error.html.twig', ['message' => $e->getMessage()]);
    }
}
```

### **Controller Return Types**

Here’s a list of common return types for controller methods:

- `RedirectResponse`: To redirect to another URL.
- `RedirectToRoute`: To redirect to another route.
- `Response`: To render a Twig template or return other types of content.

### 14. **Routing with Parameters and Default Values**

Symfony allows defining routes with optional parameters and default values.

```php
#[Route("/user/{id}", name="user_show", defaults={"id"=1})]
public function showUser($id)
{
    return $this->render('user/show.html.twig', ['id' => $id]);
}
```

---
### **II. Form**

Symfony's **Form** component simplifies handling user inputs and processing forms. A **Form** maps form fields to data models (entities or DTOs) and manages validation.

#### Responsibilities:

- Generate HTML forms.
- Handle form submissions and map data to objects.
- Validate input data.

#### Example:

**Form Class:**

```php
namespace App\Form;

use App\Entity\User;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\Extension\Core\Type\TextType;

class UserType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('name', TextType::class, [])
            ->add('email', TextType::class);
    }
}
```

**Controller Using a Form:**

```php
use App\Entity\User;
use App\Form\UserType;
use Symfony\Component\HttpFoundation\Request;

public function createUser(Request $request): Response
{
    $user = new User();
    $form = $this->createForm(UserType::class, $user);
    $form->handleRequest($request);

    if ($form->isSubmitted() && $form->isValid()) {
		...
    }

    return $this->render('user/new.html.twig', [
        'form' => $form->createView(),
    ]);
}
```

In Symfony, the Form component provides various types of form fields, each with a set of attributes that can be customized to control their behavior and appearance. Below is a list of common form types along with their attributes:

### 1. **TextType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `class`, `placeholder`).
    - `disabled`: Disables the field.
    - `trim`: Whether to trim the value.
    - `max_length`: The maximum length of the input.
- Example:
    
    ```php
    $builder->add('name', TextType::class, [
        'label' => 'Full Name',
        'required' => true,
        'attr' => ['placeholder' => 'Enter your name', 'class' => 'form-control'],
    ]);
    ```
    

### 2. **TextareaType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `rows`, `cols`).
    - `disabled`: Disables the field.
    - `trim`: Whether to trim the value.
- Example:
    
    ```php
    $builder->add('description', TextareaType::class, [
        'label' => 'Description',
        'attr' => ['rows' => 5, 'class' => 'form-control'],
    ]);
    ```
    

### 3. **IntegerType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `min`, `max`).
    - `min`: Minimum value allowed.
    - `max`: Maximum value allowed.
    - `disabled`: Disables the field.
- Example:
    
    ```php
    $builder->add('age', IntegerType::class, [
        'label' => 'Age',
        'attr' => ['min' => 18, 'max' => 100],
    ]);
    ```
    

### 4. **ChoiceType**

- **Attributes**:
    - `choices`: A list of available choices.
    - `multiple`: Whether multiple selections are allowed.
    - `expanded`: Whether the choices are rendered as checkboxes or radio buttons.
    - `preferred_choices`: Choices that should be displayed first.
    - `label`: The label for the field.
    - `placeholder`: An empty choice for the dropdown.
    - `disabled`: Disables the field.
- Example:
    
    ```php
    $builder->add('gender', ChoiceType::class, [
        'label' => 'Gender',
        'choices' => [
            'Male' => 'male',
            'Female' => 'female',
            'Other' => 'other',
        ],
        'expanded' => true, // Use checkboxes
        'multiple' => true, // Multiple selections
    ]);
    ```
    

### 5. **BooleanType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes.
    - `expanded`: Whether the options are rendered as checkboxes or radio buttons.
    - `value`: The value associated with `true`.
    - `label_true`: The label for the `true` option.
    - `label_false`: The label for the `false` option.
- Example:
    
    ```php
    $builder->add('is_active', BooleanType::class, [
        'label' => 'Is Active',
        'value' => true,
        'label_true' => 'Yes',
        'label_false' => 'No',
    ]);
    ```
    

### 6. **DateType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `widget`: The widget to use for the date input (e.g., `single_text`, `text`, `choice`).
    - `input`: The input format for the date (`datetime`, `timestamp`, `string`).
    - `format`: The format for the date display.
- Example:
    
    ```php
    $builder->add('birth_date', DateType::class, [
        'label' => 'Date of Birth',
        'widget' => 'single_text',
        'format' => 'yyyy-MM-dd',
    ]);
    ```
    

### 7. **TimeType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `widget`: The widget to use for the time input (e.g., `single_text`, `text`, `choice`).
    - `input`: The input format for the time (`datetime`, `timestamp`, `string`).
    - `minutes`: An array of minute steps to use.
- Example:
    
    ```php
    $builder->add('event_time', TimeType::class, [
        'label' => 'Event Time',
        'widget' => 'single_text',
    ]);
    ```
    

### 8. **DateTimeType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `widget`: The widget to use for the date and time input.
    - `input`: The input format for the date and time (`datetime`, `timestamp`, `string`).
    - `format`: The format for the display of both date and time.
- Example:
    
    ```php
    $builder->add('appointment_datetime', DateTimeType::class, [
        'label' => 'Appointment Date and Time',
        'widget' => 'single_text',
        'format' => 'yyyy-MM-dd HH:mm',
    ]);
    ```
    

### 9. **FileType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `accept`).
    - `multiple`: Whether multiple files can be uploaded.
    - `mapped`: Whether the field is mapped to an entity property.
- Example:
    
    ```php
    $builder->add('document', FileType::class, [
        'label' => 'Upload Document',
        'attr' => ['accept' => 'application/pdf'],
        'multiple' => true,
    ]);
    ```
    

### 10. **EmailType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `placeholder`).
    - `pattern`: The pattern for validating the email format.
- Example:
    
    ```php
    $builder->add('email', EmailType::class, [
        'label' => 'Email Address',
        'required' => true,
        'attr' => ['placeholder' => 'Enter your email'],
    ]);
    ```
    

### 11. **UrlType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `placeholder`).
    - `pattern`: The pattern for validating the URL format.
- Example:
    
    ```php
    $builder->add('website', UrlType::class, [
        'label' => 'Website URL',
        'attr' => ['placeholder' => 'https://example.com'],
    ]);
    ```
    

### 12. **PasswordType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes (e.g., `autocomplete`, `placeholder`).
- Example:
    
    ```php
    $builder->add('password', PasswordType::class, [
        'label' => 'Password',
        'required' => true,
        'attr' => ['placeholder' => 'Enter your password'],
    ]);
    ```
    

### 13. **HiddenType**

- **Attributes**:
    - `label`: The label for the field.
    - `required`: Whether the field is required or not.
    - `attr`: Custom HTML attributes.
- Example:
    
    ```php
    $builder->add('user_id', HiddenType::class, [
        'data' => $userId, // Pre-populated value
    ]);
    ```

---

### **III. View**

The **View** refers to the presentation layer of your application. In Symfony, it's commonly implemented using the **Twig** templating engine. The **View** is responsible for rendering the data passed from the controller into a user-friendly format.

#### Responsibilities:

- Display data in a specific format (HTML, JSON, etc.).
- Keep the presentation logic separate from application logic.

#### Example (Twig Template):

**`templates/example/index.html.twig`**

```twig
<h1>{{ message }}</h1>
```

Here is a comprehensive list of Twig syntax, including basic features and advanced functionalities:

### 1. **Variables**

- **Displaying Variables**:
    
    ```twig
    {{ variable }}
    ```
    
- **Default Value**:
    
    ```twig
    {{ variable | default('default_value') }}
    ```
    
- **Escaping Output** (automatically escapes):
    
    ```twig
    {{ variable | e }}
    ```
    
- **Raw Output (no escaping)**:
    
    ```twig
    {{ variable | raw }}
    ```
    

### 2. **Filters**

- **Lowercase**:
    
    ```twig
    {{ 'HELLO' | lower }}  {# hello #}
    ```
    
- **Uppercase**:
    
    ```twig
    {{ 'hello' | upper }}  {# HELLO #}
    ```
    
- **Length**:
    
    ```twig
    {{ 'hello' | length }}  {# 5 #}
    ```
    
- **Join**:
    
    ```twig
    {{ array | join(', ') }}  {# "apple, banana, cherry" #}
    ```
    

### 3. **Test Operators**

- **Checking Type**:
    
    ```twig
    {% if variable is string %}
        {# do something #}
    {% endif %}
    ```
    
- **Empty**:
    
    ```twig
    {% if variable is empty %}
        {# do something #}
    {% endif %}
    ```
    

### 4. **Control Structures**

- **If Statement**:
    
    ```twig
    {% if condition %}
        {# do something #}
    {% elseif other_condition %}
        {# do something else #}
    {% else %}
        {# fallback #}
    {% endif %}
    ```
    
- **For Loop**:
    
    ```twig
    {% for item in items %}
        {{ item }}
    {% else %}
        {# No items #}
    {% endfor %}
    ```
    
- **Loop Index**:
    
    ```twig
    {% for item in items %}
        {{ loop.index }}  {# 1-based index #}
        {{ loop.index0 }} {# 0-based index #}
    {% endfor %}
    ```
    

### 5. **Filters**

- **Date Format**:
    
    ```twig
    {{ 'now' | date('Y-m-d H:i') }}
    ```
    
- **Currency**:
    
    ```twig
    {{ 1234.56154 | number_format(2) }}  {# 1234.56 #}
    ```
    
### 11. **Comments**

- **Twig Comments**:
    
    ```twig
    {# This is a comment #}
    ```
    
- **Block Comments**:
    
    ```twig
    {% comment %}
        This is a block comment.
    {% endcomment %}
    ```
    

### 12. **Filters for Working with Strings and Arrays**

- **String Replace**:
    
    ```twig
    {{ 'hello' | replace({'e': 'a'}) }}  {# hallo #}
    ```
    
- **Array Slice**:
    
    ```twig
    {{ array | slice(1, 2) }}  {# from index 1, take 2 elements #}
    ```
    

### 13. **Other Useful Filters**

- **Merge Arrays**:
    
    ```twig
    {{ array1 | merge(array2) }}
    ```
    
- **Unique Array**:
    
    ```twig
    {{ array | unique }}
    ```
    
### 16. **Paths**

- **URL Generation**:
    
    ```twig
    {{ path('route_name') }}
    ```
    
- **Asset Path**:
    
    ```twig
    {{ asset('image.png') }}
    ```
    

### 17. **Ternary Operator**

```twig
{{ condition ? 'true_value' : 'false_value' }}
```

---

### **IV. Repository**

A **Repository** is a class that interacts with the database. In Symfony (with Doctrine ORM), repositories are responsible for querying the database for entities.

#### Responsibilities:

- Retrieve entities from the database.
- Implement complex database queries.
- Abstract database logic from the controller.

#### Example:

**Repository Class:**

```php
namespace App\Repository;

use App\Entity\User;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;

class UserRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, User::class);
    }

    public function findByEmail(string $email): ?User
    {
        return $this->findOneBy(['email' => $email]);
    }
}
```

**Controller Using a Repository:**

```php
use App\Repository\UserRepository;

public function showUser(UserRepository $userRepository, string $email)
{
    $user = $userRepository->findByEmail($email);

    return $this->render('user/show.html.twig', [
        'user' => $user,
    ]);
}
```

In Symfony, repositories are used to interact with the database via Doctrine ORM. They provide methods for querying and persisting entities. Below is an overview of the common repository syntax, methods, and best practices:

### 1. **Basic Repository Syntax**

A repository is typically a PHP class that extends `ServiceEntityRepository` and provides methods for retrieving and saving entities. Here's the structure of a repository:

```php
// src/Repository/YourEntityRepository.php
namespace App\Repository;

use App\Entity\YourEntity;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;

class YourEntityRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, YourEntity::class);
    }

    // Custom query methods go here
}
```

### 2. **Common Repository Methods**

Symfony provides several common methods for interacting with the database, and you can also define custom query methods in the repository.
#### b. **Custom Query Methods**

You can define custom query methods to perform more complex operations.

```php
// In YourEntityRepository.php
public function findActiveUsers()
{
    return $this->createQueryBuilder('u')
        ->where('u.username = ":name"')
        ->setParameter('name', "hafedh")
        ->getQuery()
        ->getResult();
}
```

- **createQueryBuilder()**: This allows you to build more complex queries using Doctrine’s DQL (Doctrine Query Language).
    
    ```php
    $queryBuilder = $this->createQueryBuilder('e')
        ->where('e.status = :status')
        ->setParameter('status', 'active')
        ->orderBy('e.createdAt', 'DESC');
    $result = $queryBuilder->getQuery()->getResult();
    ```
    
- **createQuery()**: You can also use raw DQL for more complex queries:
    
    ```php
    $query = $this->getEntityManager()->createQuery('SELECT e FROM App\Entity\YourEntity e WHERE e.status = :status');
    $query->setParameter('status', 'active');
    $result = $query->getResult();
    ```
    

#### c. **Counting Entities**

If you need to count entities based on a specific condition:

```php
public function countActiveUsers()
{
    return $this->createQueryBuilder('u')
        ->select('COUNT(u.id)')
        ->where('u.isActive = :active')
        ->setParameter('active', true)
        ->getQuery()
        ->getSingleScalarResult();
}
```

#### d. **Paginated Results**

You can implement pagination using `QueryBuilder` with `setMaxResults` and `setFirstResult`.

```php
public function getPaginatedResults($page, $limit)
{
    return $this->createQueryBuilder('e')
        ->setFirstResult(($page - 1) * $limit)
        ->setMaxResults($limit)
        ->getQuery()
        ->getResult();
}
```

### 3. **Querying with `Doctrine\ORM\QueryBuilder`**

The `QueryBuilder` allows you to build complex queries using a fluent interface. Here’s an example of using `QueryBuilder` for complex queries:

```php
public function findEntitiesByStatusAndDate($status, \DateTime $date)
{
    return $this->createQueryBuilder('e')
        ->where('e.status = :status')
        ->andWhere('e.date > :date')
        ->setParameters([
            'status' => $status,
            'date' => $date
        ])
        ->orderBy('e.date', 'ASC')
        ->getQuery()
        ->getResult();
}
```

### 4. **Repository Methods for Inserting/Updating Entities**

You can use the `EntityManager` to handle persistence (inserting, updating, and removing entities).

- **Persisting an Entity**:
    
    ```php
    public function saveEntity(YourEntity $entity)
    {
        $this->_em->persist($entity);
        $this->_em->flush();
    }
    ```
    
- **Removing an Entity**:
    
    ```php
    public function removeEntity(YourEntity $entity)
    {
        $this->_em->remove($entity);
        $this->_em->flush();
    }
    ```
    
### Summary of Repository Methods

- **find($id)**: Find a single entity by its ID.
- **findAll()**: Find all entities of the repository’s type.
- **findBy($criteria, $orderBy, $limit, $offset)**: Find entities matching certain criteria.
- **findOneBy($criteria)**: Find a single entity matching certain criteria.
- **createQueryBuilder($alias)**: Create a `QueryBuilder` for complex queries.
- **getRepository($entityName)**: Retrieve the repository for an entity.
- **persist($entity)**: Persist an entity (save it to the database).
- **flush()**: Apply all changes to the database.
- **remove($entity)**: Remove an entity from the database.
- **matching(Criteria $criteria)**: Retrieve entities matching a given `Criteria` object.

Doctrine repositories are powerful tools for querying and managing entities in Symfony, and understanding the various query methods allows you to leverage their full potential.

---

### Summary of Roles

|Component|Responsibility|
|---|---|
|**Controller**|Handles requests, interacts with the model, returns response.|
|**Form**|Manages user input, form creation, and validation.|
|**View**|Renders data to the user (HTML, JSON, etc.).|
|**Repository**|Handles database interactions and queries.|

Together, these components align with the **Model-View-Controller (MVC)** pattern, ensuring separation of concerns and maintainable code.

![[Pasted image 20250111142416.png]]