### 1. **Entities (Doctrine ORM Fields)**

Define your entities with annotations or attributes. Common field types include:

| Field Name      | Doctrine Type                                         | Description           |
| --------------- | ----------------------------------------------------- | --------------------- |
| `id`            | `@ORM\Id` and `@ORM\GeneratedValue`                   | Primary key.          |
| `stringField`   | `@ORM\Column(type="string", length=255)`              | String field.         |
| `textField`     | `@ORM\Column(type="text")`                            | Long text.            |
| `integerField`  | `@ORM\Column(type="integer")`                         | Integer value.        |
| `floatField`    | `@ORM\Column(type="float")`                           | Floating-point value. |
| `booleanField`  | `@ORM\Column(type="boolean")`                         | Boolean value.        |
| `datetimeField` | `@ORM\Column(type="datetime")`                        | Date and time.        |
| `relationField` | `@ORM\ManyToOne`, `@ORM\OneToMany`, `@ORM\ManyToMany` | Relationships.        |

{3}
{3, 5}
{5,}

Example entity:

```php
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
class Product
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private ?int $id = null;

    #[ORM\Column(type: 'string', length: 255)]
    private string $name;

    #[ORM\Column(type: 'float')]
    private float $price;

    #[ORM\Column(type: 'boolean')]
    private bool $isActive;

    #[ORM\Column(type: 'datetime')]
    private \DateTime $createdAt;

    #[ORM\ManyToOne(targetEntity: Category::class)]
    private ?Category $category;
}
```

---

### 2. **Controller Routes**

Define routes using PHP attributes in controllers.

| Route Configuration | Syntax Example                               |
| ------------------- | -------------------------------------------- |
| `Path`              | `#[Route('/path/{id}', name: 'route_name')]` |
| `Methods`           | `methods: ['GET', 'POST']`                   |
| `Requirements`      | `requirements: ['id' => '\d+']`              |
| `Defaults`          | `defaults: ['id' => 1]`                      |

**REGEX** : 
	\d == [0-9]
	 \w == [a-zA-Z0-9-_ ]
	 + == 1 -> +oo
	 * == 0 -> +oo
	 \s
	 \S
	 \W
	Exemples: \d{2} \d{3} \d{3}

Example:

```php
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class ProductController extends AbstractController
{
    #[Route('/product/{id}', name: 'product_show', methods: ['GET'], requirements: ['id' => '\d+'])]
    public function show(int $id): Response
    {
        return new Response("Product ID: $id");
    }
}
```

---

### 3. **Form Fields**

Symfony provides various field types for forms:

|Field Type|Usage Example|
|---|---|
|`TextType`|`->add('name', TextType::class)`|
|`EmailType`|`->add('email', EmailType::class)`|
|`PasswordType`|`->add('password', PasswordType::class)`|
|`CheckboxType`|`->add('isActive', CheckboxType::class)`|
|`ChoiceType`|`->add('category', ChoiceType::class, ['choices' => [...]])`|
|`FileType`|`->add('file', FileType::class)`|

Example:

```php
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\EmailType;

class ProductType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            ->add('name', TextType::class)
            ->add('email', EmailType::class);
    }
}
```

---
### 4. **Twig Templates**

Basic syntax for rendering data:

```twig
<h1>{{ product.name }}</h1>
<p>{{ product.price | number_format(2) }}</p>
```

Render a route:

```twig
<a href="{{ path('product_show', {id: product.id}) }}">View</a>
```

---
### **1. What is an Entity?**

An entity in Symfony is a PHP class that maps to a table in the database. The fields in the entity class correspond to the columns in the database table. Symfony uses Doctrine ORM to handle these mappings.

---

### **2. Entity Structure**

An entity typically includes:

- Properties: Corresponding to database columns.
- Annotations/Attributes: To map the properties to database fields.
- Getter and Setter Methods: To access and modify the property values.

**Example:**

```php
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
#[ORM\Table(name: "products")]
class Product
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: "integer")]
    private ?int $id = null;

    #[ORM\Column(type: "string", length: 255)]
    private string $name;

    #[ORM\Column(type: "decimal", precision: 10, scale: 2)]
    private float $price;

    #[ORM\Column(type: "datetime")]
    private \DateTime $createdAt;

    // Getters and setters...
}
```

---

### **3. Common Field Types**

Here are some common Doctrine column types and their usage:

|Field Type|Syntax Example|Notes|
|---|---|---|
|`string`|`#[ORM\Column(type: "string", length: 255)]`|For short text fields. Max length: 255.|
|`text`|`#[ORM\Column(type: "text")]`|For longer text fields. No length limit.|
|`integer`|`#[ORM\Column(type: "integer")]`|For integer values.|
|`float`|`#[ORM\Column(type: "float")]`|For decimal or floating-point values.|
|`boolean`|`#[ORM\Column(type: "boolean")]`|For true/false values.|
|`datetime`|`#[ORM\Column(type: "datetime")]`|For date and time values.|
|`date`|`#[ORM\Column(type: "date")]`|For date-only values.|
|`time`|`#[ORM\Column(type: "time")]`|For time-only values.|
|`json`|`#[ORM\Column(type: "json")]`|For storing JSON data.|
|`uuid`|`#[ORM\Column(type: "uuid", unique: true)]`|For universally unique identifiers.|
|`array`|`#[ORM\Column(type: "array")]`|For storing PHP arrays (not recommended).|

---

### **4. Relationships Between Entities**

Doctrine supports various types of relationships between entities:

#### **4.1 One-to-One Relationship**

Maps a single entity to another entity.

**Example:**

```php
#[ORM\Entity]
class User
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: "integer")]
    private ?int $id = null;

    #[ORM\OneToOne(targetEntity: Profile::class, cascade: ["persist", "remove"])]
    #[ORM\JoinColumn(nullable: false)]
    private Profile $profile;
}
```

#### **4.2 One-to-Many Relationship**

One entity has a collection of another entity.

**Example:**

```php
#[ORM\Entity]
class Category
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: "integer")]
    private ?int $id = null;

    #[ORM\OneToMany(mappedBy: "category", targetEntity: Product::class, cascade: ["persist"])]
    private Collection $products;

    public function __construct()
    {
        $this->products = new ArrayCollection();
    }
}
```

#### **4.3 Many-to-One Relationship**

Maps multiple entities to one entity.

**Example:**

```php
#[ORM\Entity]
class Product
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: "integer")]
    private ?int $id = null;

    #[ORM\ManyToOne(targetEntity: Category::class, inversedBy: "products")]
    private Category $category;
}
```

#### **4.4 Many-to-Many Relationship**

Many entities are related to many others.

**Example:**

```php
#[ORM\Entity]
class Student
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: "integer")]
    private ?int $id = null;

    #[ORM\ManyToMany(targetEntity: Course::class, inversedBy: "students")]
    #[ORM\JoinTable(name: "student_courses")]
    private Collection $courses;

    public function __construct()
    {
        $this->courses = new ArrayCollection();
    }
}
```

---

### **7. Best Practices for Entities**

1. **Always Use Typed Properties:** Symfony 6 requires PHP 8, so use typed properties.
2. **Avoid Business Logic in Entities:** Keep entities simple; place business logic in services.
3. **Use `ArrayCollection` for Relationships:** Initialize collections in the constructor.
4. **Use Validation Annotations/Attributes:** Define constraints for entity fields.

---

