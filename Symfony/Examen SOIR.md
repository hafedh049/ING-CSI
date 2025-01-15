## QCM

### A
	1 - a
	2 - a
	3 - a
	4 - a
	5 - b

### B
### 1
![[Pasted image 20241227163029.png]]

```php
echo '<dl>';
foreach ($animaux as $animal) {
    if ($animal['type'] == 'mammifère') {
        echo '<dt>' . $animal['nom'] . '</dt>';
        echo '<dd>' . $animal['definition'] . '</dd>';
    }
}
echo '</dl>';
```

```twig
<dl>
    {% for animal in animaux %}
        {% if animal.type == 'mammifère' %}
            <dt>{{ animal.nom }}</dt>
            <dd>{{ animal.definition }}</dd>
        {% endif %}
    {% endfor %}
</dl>
```

### 2
![[Pasted image 20241227163157.png]]

2-1-a -> loverById
2-a-b -> loverById

2-2
```php
#[Route('/mylover/{id}', name: 'loverById', requirements: ['id' => '\d'], defaults: ["id" => 5])]
public function loverById(int $id) { ... }
```

## PROBLEME

![[Pasted image 20241227163517.png]]

>[!note]
>**Primary key migrations** are always from string to weak in symfony migrations are done using fields for example 'Category' & 'Product'. A category contains 0 to many products right ? so this means that the Category entity contains an array or a collection called products that contains the products related to category and the Product table or entity will contain the primary key of the Category

### A
![[Pasted image 20241227164825.png]]

```php
<?php

namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

use Doctrine\Common\Collections\Collection; 
use Doctrine\Common\Collections\ArrayCollection;

<?php

namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;
use Doctrine\Common\Collections\Collection;
use Doctrine\Common\Collections\ArrayCollection;

#[ORM\Entity]
class Reservation
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column]
    private ?int $id = null;

    #[ORM\Column(type: 'datetime')]
    private ?\DateTimeInterface $dateR = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $etatR = null;

    #[ORM\ManyToOne(targetEntity: User::class, inversedBy: 'reservations')]
    #[ORM\JoinColumn(nullable: false)]
    private ?User $user = null;

    #[ORM\ManyToOne(targetEntity: Projection::class, inversedBy: 'reservations')]
    #[ORM\JoinColumn(nullable: false)]
    private ?Projection $projection = null;

    // Getters and Setters
    public function getId(): ?int
    {
        return $this->id;
    }

    public function getDateR(): ?\DateTimeInterface
    {
        return $this->dateR;
    }

    public function setDateR(\DateTimeInterface $dateR): self
    {
        $this->dateR = $dateR;

        return $this;
    }

    public function getEtatR(): ?string
    {
        return $this->etatR;
    }

    public function setEtatR(string $etatR): self
    {
        $this->etatR = $etatR;

        return $this;
    }

    public function getUser(): ?User
    {
        return $this->user;
    }

    public function setUser(?User $user): self
    {
        $this->user = $user;

        return $this;
    }

    public function getProjection(): ?Projection
    {
        return $this->projection;
    }

    public function setProjection(?Projection $projection): self
    {
        $this->projection = $projection;

        return $this;
    }
}

#[ORM\Entity]
class Projection
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column]
    private ?int $id = null;

    #[ORM\Column(type: 'datetime')]
    private ?\DateTimeInterface $dateProjection = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $salle = null;

    #[ORM\Column(type: 'integer')]
    private ?int $nbrPlaces = null;

    #[ORM\ManyToOne(targetEntity: Film::class, inversedBy: 'projections')]
    #[ORM\JoinColumn(nullable: false)]
    private ?Film $film = null;

    #[ORM\OneToMany(mappedBy: 'projection', targetEntity: Reservation::class)]
    private Collection $reservations;

    public function __construct()
    {
        $this->reservations = new ArrayCollection();
    }

    // Getters and Setters
    public function getId(): ?int
    {
        return $this->id;
    }

    public function getDateProjection(): ?\DateTimeInterface
    {
        return $this->dateProjection;
    }

    public function setDateProjection(\DateTimeInterface $dateProjection): self
    {
        $this->dateProjection = $dateProjection;

        return $this;
    }

    public function getSalle(): ?string
    {
        return $this->salle;
    }

    public function setSalle(string $salle): self
    {
        $this->salle = $salle;

        return $this;
    }

    public function getNbrPlaces(): ?int
    {
        return $this->nbrPlaces;
    }

    public function setNbrPlaces(int $nbrPlaces): self
    {
        $this->nbrPlaces = $nbrPlaces;

        return $this;
    }

    public function getFilm(): ?Film
    {
        return $this->film;
    }

    public function setFilm(?Film $film): self
    {
        $this->film = $film;

        return $this;
    }

    /**
     * @return Collection<int, Reservation>
     */
    public function getReservations(): Collection
    {
        return $this->reservations;
    }

    public function addReservation(Reservation $reservation): self
    {
        if (!$this->reservations->contains($reservation)) {
            $this->reservations->add($reservation);
            $reservation->setProjection($this);
        }

        return $this;
    }

    public function removeReservation(Reservation $reservation): self
    {
        if ($this->reservations->removeElement($reservation)) {
            if ($reservation->getProjection() === $this) {
                $reservation->setProjection(null);
            }
        }

        return $this;
    }
}

#[ORM\Entity]
class Film
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column]
    private ?int $id = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $titre = null;

    #[ORM\Column(type: 'datetime')]
    private ?\DateTimeInterface $dateCreation = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $genre = null;

    #[ORM\OneToMany(mappedBy: 'film', targetEntity: Projection::class)]
    private Collection $projections;
}

#[ORM\Entity]
class User
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column]
    private ?int $id = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $nom = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $prenom = null;

    #[ORM\Column(type: 'string', length: 255, unique: true)]
    private ?string $email = null;

    #[ORM\Column(type: 'string', length: 255)]
    private ?string $password = null;

    #[ORM\OneToMany(mappedBy: 'user', targetEntity: Reservation::class)]
    private Collection $reservations;
}
```

### B
![[Pasted image 20241227170032.png]]

```php
<?php

namespace App\Controller;

use App\Repository\ReservationRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class ReservationController extends AbstractController
{
    #[Route('/reservations', name: 'app_reservation_list')]
    public function listR($reservationRepository $reservationRepository): Response
    {
        $reservations = $reservationRepository->findAll();

        return $this->render('Templates/R/reservation/listR.html.twig', [
            'reservations' => $reservations,
        ]);
    }
}

```

```twig
{# templates/reservation/listR.html.twig #}
{% extends 'base.html.twig' %}

{% block title %}Liste des Réservations{% endblock %}

{% block body %}
    <h1>Liste des Réservations</h1>

    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Date</th>
                <th>État</th>
                <th>Utilisateur</th>
                <th>Film</th>
                <th>Salle</th>
            </tr>
        </thead>
        <tbody>
        {% for reservation in reservations %}
            <tr>
                <td>{{ reservation.id }}</td>
                <td>{{ reservation.dateR | date('Y-m-d H:i') }}</td>
                <td>{{ reservation.etatR }}</td>
                <td>{{ reservation.user.nom }} {{ reservation.user.prenom }}</td>
                <td>{{ reservation.projection.film.titre }}</td>
                <td>{{ reservation.projection.salle }}</td>
            </tr>
        {% else %}
            <tr>
                <td colspan="6">Aucune réservation trouvée</td>
            </tr>
        {% endfor %}
        </tbody>
    </table>
{% endblock %}
```
### C

![[Pasted image 20241227165414.png]]

```php
<?php

namespace App\Controller;

use App\Entity\Reservation;
use App\Form\RForm;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class ReservationController extends AbstractController
{
    #[Route('/reservation/add', name: 'app_reservation_add')]
    public function addR(Request $request, EntityManagerInterface $entityManager): Response
    {
        $reservation = new Reservation();
        $reservation->setDateR(new \DateTime());
        $reservation->setEtatR('pending'); 

        // Create form
        $form = $this->createForm(RForm::class, $reservation);
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            // Save the reservation
            $entityManager->persist($reservation);
            $entityManager->flush();

            // Add flash message
            $this->addFlash('success', 'Réservation ajoutée avec succès');

            // Redirect to list
            return $this->redirectToRoute('app_reservation_list');
        }

        // Display the form
        return $this->render('reservation/addR.html.twig', [
            'form' => $form->createView(),
        ]);
    }
}

// In src/Form/RForm.php
namespace App\Form;

use App\Entity\Reservation;
use App\Entity\Projection;
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class RForm extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('projection', EntityType::class, [
                'class' => Projection::class,
                'choice_label' => function ($projection) {
                    return $projection->getFilm()->getTitre() . ' - ' . 
                           $projection->getDateProjection()->format('d/m/Y H:i') . ' - ' . 
                           $projection->getSalle();
                },
                'label' => 'Séance',
                'required' => true,
            ]);
    }

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => Reservation::class,
        ]);
    }
}
```

```twig
{# templates/reservation/addR.html.twig #}
{% extends 'base.html.twig' %}

{% block title %}Nouvelle Réservation{% endblock %}

{% block body %}
    <div class="container">
        <h1>Nouvelle Réservation</h1>

        {{ form_start(form) }}
            {{ form_row(form.projection) }}

            <button type="submit" class="btn btn-primary">
                Réserver
            </button>

            <a href="{{ path('app_reservation_list') }}" class="btn btn-secondary">
                Retour à la liste
            </a>
        {{ form_end(form) }}
    </div>
{% endblock %}
```

### D

```php
<?php

namespace App\Form;

use App\Entity\Film;
use App\Entity\Projection;
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\DateTimeType;
use Symfony\Component\Form\Extension\Core\Type\IntegerType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class PForm extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('dateProjection', DateTimeType::class, [
                'label' => 'Date et heure de la projection',
                'widget' => 'single_text',
                'required' => true,
            ])
            ->add('salle', TextType::class, [
                'label' => 'Salle',
                'required' => true,
            ])
            ->add('nbrPlaces', IntegerType::class, [
                'label' => 'Nombre de places',
                'required' => true,
                'attr' => [
                    'min' => 1,
                ],
            ])
            ->add('film', EntityType::class, [
                'class' => Film::class,
                'choice_label' => 'titre',
                'label' => 'Film',
                'required' => true,
            ]);
    }

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => Projection::class,
        ]);
    }
}
```

### E
```php
<?php

namespace App\Repository;

use App\Entity\Reservation;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;

class ReservationRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, Reservation::class);
    }

    public function nombreRéservationsAcceptées($film): array
    {
        return $this->createQueryBuilder('r')
            ->select('p.salle as salle, COUNT(r.id) as nombreReservations')
            ->join('r.projection', 'p')
            ->join('p.film', 'f')
            ->where('f.titre = :film')
            ->andWhere('r.etatR = :etat')
            ->groupBy('p.salle')
            ->setParameter('film', $film)
            ->setParameter('etat', 'accepted')
            ->getQuery()
            ->getResult();
    }

    // Alternative using pure DQL
    public function nombreRéservationsAcceptéesDQL($film): array
    {
        $em = $this->getEntityManager();
        
        $query = $em->createQuery(
            'SELECT p.salle as salle, COUNT(r.id) as nombreReservations
             FROM App\Entity\Reservation r
             JOIN r.projection p
             JOIN p.film f
             WHERE f.titre = :film
             AND r.etatR = :etat
             GROUP BY p.salle'
        )
        ->setParameter('film', $film)
        ->setParameter('etat', 'accepted');

        return $query->getResult();
    }
}
```