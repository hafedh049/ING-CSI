# A - QCM ($6 \text{pts}$)

![[Pasted image 20250115082252.png]]

![[Pasted image 20250115082327.png]]

![[Pasted image 20250115082353.png]]

![[Pasted image 20250115082416.png]]

![[Pasted image 20250115082506.png]]

![[Pasted image 20250115082542.png]]

# B - Question / Réponse ($4 \text{pts}$)

![[Pasted image 20250115082630.png]]

```php
<table>
    {% for relation in mesRelations %}
        {% if relation.getDuree > 3 %}
            <tr>
                <td>{{ relation.getNom() }}</td>
                <td><img src="{{ relation.getPhoto() }}"></td>
            </tr>
        {% endif %}
    {% endfor %}
</table>
```

![[Pasted image 20250115082733.png]]
![[Pasted image 20250115082955.png]]
```php
exById("3");
```
![[Pasted image 20250115083036.png]]
```php
exById("BradleyCooper");
```
![[Pasted image 20250115083125.png]]
```php
#[Route('/myEx/{id}', name: 'exById', requirements: ['id' => '\d+'])]
```

# Probléme ($10 \text{pts}$)

![[Pasted image 20250115083344.png]]
![[Pasted image 20250115083600.png]]

```php
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity(repositoryClass: ServiceRepository::class)]
class Service
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private ?$id = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?$nom = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?String $type;

    #[ORM\OneToMany(mappedBy: 'service', targetEntity: Reservation::class)]
    private Collection $reservations;

	function __construct(){
		$this->reservations = new ArrayCollection();
	}

    // Add getters and setters as needed
}
```

```php
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
class Reservation
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private $id;

    #[ORM\Column(type: 'date')]
    private $dateR;

    #[ORM\Column(type: 'string', length: 255)]
    private $etatR;

    #[ORM\ManyToOne(targetEntity: Service::class, inversedBy: 'reservations')]
    private $service;

    #[ORM\ManyToOne(targetEntity: Animal::class, inversedBy: 'reservations')]
    #[ORM\JoinColumn(nullable: false)]
    private $animal;

    // Add getters and setters as needed
}
```

![[Pasted image 20250115084140.png]]

```php
namespace App\Controller;

use App\Repository\ReservationRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class ReservationController extends AbstractController
{
    public function listR(ReservationRepository $repo): Response
    {
        // Liste des réservations
        $reservations = $repo->findAll();

        return $this->render('R/listR.html.twig', [
            'listeR' => $reservations,
        ]);
    }
}
```

![[Pasted image 20250115084250.png]]

```twig
<h1>Liste des réservations</h1>

<table border="1">
    <thead>
        <tr>
            <th>Nom de l'animal</th>
            <th>Nom du service</th>
            <th>Date de réservation</th>
            <th>État</th>
        </tr>
    </thead>
    <tbody>
        {% for reservation in listeR %}
            <tr>
                <td>{{ reservation.animal.nom }}</td>
                <td>{{ reservation.service.nom }}</td>
                <td>{{ reservation.dateR | date('Y-m-d') }}</td>
                <td>{{ reservation.etatR }}</td>
            </tr>
        {% endfor %}
    </tbody>
</table>
```

![[Pasted image 20250115084404.png]]

```php
namespace App\Controller;

use App\Entity\Animal;
use App\Form\AnimalType;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

class AnimalController extends AbstractController
{
	#[Route('/addA', name: 'addA')]
    public function addA(Request $request, EntityManagerInterface $entityManager): Response
    {
        // Création d'un animal
        $animal = new Animal();

        // Création d'un formulaire
        $form = $this->createForm(AnimalType::class, $animal);

        // Traitement de la soumission du formulaire
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            // Enregistrer l'animal dans la base de données
            $entityManager->persist($animal);
            $entityManager->flush();

            // Rediriger vers la liste des animaux
            return $this->redirectToRoute('listeARoute');
        }

        // Affichage du formulaire
        return $this->render('A/addA.html.twig', [
            'form' => $form->createView(),
        ]);
    }
}
```

![[Pasted image 20250115084544.png]]

```twig
<h1>Add Animal</h1>

<form action="{{ path('addA') }}" method="POST">
    {{ form_start(form) }}
    {{ form_widget(form) }}
    <button type="submit">Submit</button>
    {{ form_end(form) }}
</form>
```

```php
/*
	<div>
	    {{ form_label(form.nom) }}
	    {{ form_widget(form.nom) }}
	    {{ form_errors(form.nom) }}
	</div>
*/
```

![[Pasted image 20250115085042.png]]

```php
<?php

namespace App\Form;

use App\Entity\Reservation;
use App\Entity\Service;
use App\Entity\Animal;
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class RForm extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            ->add('dateR', DateTimeType, [
                'widget' => 'single_text', // For date selection
                'label' => 'Reservation Date',
            ])
            ->add('etatR', CheckBoxType, [
                'label' => 'Reservation Status',
            ])
            ->add('service', EntityType::class, [
                'class' => Service::class,
                'choice_label' => 'nom', // Display the name of the service
                'label' => 'Service',
                'placeholder' => 'Choose a service',
            ])
            ->add('animal', EntityType::class, [
                'class' => Animal::class,
                'choice_label' => 'nom', // Display the name of the animal
                'label' => 'Animal',
                'placeholder' => 'Choose an animal',
            ]);
    }

    public function configureOptions(OptionsResolver $resolver): void
    {
        $resolver->setDefaults([
            'data_class' => Reservation::class,
        ]);
    }
}
```

![[Pasted image 20250115085439.png]]

```php
<?php

namespace App\Controller;

use App\Repository\UserRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class UserController extends AbstractController
{
    /**
     * @Route("/user/{id}", name="user_show", requirements={"id"="\d+"})
     */
    public function showUser(int $id, UserRepository $userRepository): Response
    {
        // Retrieve the user by their id
        $user = $userRepository->find($id);

        // If the user is not found, throw an exception or return a 404 response
        if (!$user) {
            throw $this->createNotFoundException('The user does not exist');
        }

        // Render the Twig template with the user details
        return $this->render('U/show.html.twig', [
            'user' => $user,
        ]);
    }
}
```