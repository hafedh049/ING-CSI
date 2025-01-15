# A - QCM

![[Pasted image 20250111145718.png]]

![[Pasted image 20250111145907.png]]

![[Pasted image 20250111145944.png]]

![[Pasted image 20250111150109.png]]

![[Pasted image 20250111150330.png]]

![[Pasted image 20250111150454.png]]

![[Pasted image 20250111150557.png]]

# B - Question / Reponse

![[Pasted image 20250111154426.png]]
```twig
{% if list_personnes | length > 0 %}
	{% for personne in list_personnes %}
		<li> {{ personne.getCin() }} </li>
 	{% endfor %}
{% else %}
	<li> Pas de personne trouvé </li>
{% endif %}
```

# C - Probleme

![[Pasted image 20250111155856.png]]

## A - 1

```php
<?php

namespace App\Entity;

use Doctrine\Common\Collections\ArrayCollection;
use Doctrine\Common\Collections\Collection;
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
class Auteur
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private int $id;

    #[ORM\Column(type: 'string', length: 255)]
    private string $nom;

    #[ORM\Column(type: 'string', length: 255)]
    private string $prenom;

    #[ORM\Column(type: 'string', length: 255, nullable: true)]
    private ?string $pseudonyme = null;

    #[ORM\OneToMany(mappedBy: 'auteur', targetEntity: Livre::class, cascade: ['persist', 'remove'])]
    private Collection $livres;

    public function __construct()
    {
        $this->livres = new ArrayCollection();
    }

    public function getId(): int
    {
        return $this->id;
    }

    public function getNom(): string
    {
        return $this->nom;
    }

    public function setNom(string $nom): self
    {
        $this->nom = $nom;
        return $this;
    }

    public function getPrenom(): string
    {
        return $this->prenom;
    }

    public function setPrenom(string $prenom): self
    {
        $this->prenom = $prenom;
        return $this;
    }

    public function getPseudonyme(): ?string
    {
        return $this->pseudonyme;
    }

    public function setPseudonyme(?string $pseudonyme): self
    {
        $this->pseudonyme = $pseudonyme;
        return $this;
    }

    public function getLivres(): Collection
    {
        return $this->livres;
    }

    public function addLivre(Livre $livre): self
    {
        if (!$this->livres->contains($livre)) {
            $this->livres->add($livre);
            $livre->setAuteur($this);
        }
        return $this;
    }

    public function removeLivre(Livre $livre): self
    {
        if ($this->livres->removeElement($livre) && $livre->getAuteur() === $this) {
            $livre->setAuteur(null);
        }
        return $this;
    }
}
```

```php
<?php

namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
class Livre
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private int $id;

    #[ORM\Column(type: 'string', length: 255)]
    private string $titre;

    #[ORM\Column(type: 'string', length: 255)]
    private string $genre;

    #[ORM\ManyToOne(targetEntity: Auteur::class, inversedBy: 'livres')]
    #[ORM\JoinColumn(nullable: false)]
    private ?Auteur $auteur = null;

    public function getId(): int
    {
        return $this->id;
    }

    public function getTitre(): string
    {
        return $this->titre;
    }

    public function setTitre(string $titre): self
    {
        $this->titre = $titre;
        return $this;
    }

    public function getGenre(): string
    {
        return $this->genre;
    }

    public function setGenre(string $genre): self
    {
        $this->genre = $genre;
        return $this;
    }

    public function getAuteur(): ?Auteur
    {
        return $this->auteur;
    }

    public function setAuteur(?Auteur $auteur): self
    {
        $this->auteur = $auteur;
        return $this;
    }
}
```

## B - 1

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use App\Repository\LivreRepository;

class LivreController extends AbstractController
{
    public function listLivreAction(LivreRepository $livreRepository): Response
    {
        $livres = $livreRepository->findAll();

        return $this->render(
            'Templates/Livre/listLivre.html.twig',
            array(
                'livres' => $livres
            )
        );
    }
}

```

## B - 2

```twig
<h1>Liste des livres</h1>
<table border="1">
    <tr>
        <th>Id</th>
        <th>Titre</th>
        <th>Genre</th>
        <th>Auteur</th>
    </tr>
    {% for livre in livres %}
        <tr>
            <td>{{ livre.id }}</td>
            <td>{{ livre.titre }}</td>
            <td>{{ livre.genre }}</td>
            <td>{{ livre.auteur.nom }}</td>
        </tr>
    {% else %}
        <tr>
            <td colspan="4">Aucun livre trouvé</td>
        </tr>
    {% endfor %}
</table>
```

## C - 1

```php
<?php

namespace App\Form;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\Extension\Core\Type\ChoiceType;
use Symfony\Component\OptionsResolver\OptionsResolver;
use App\Entity\Livre;

class LivreForm extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('titre', TextType::class, [
                'label' => 'Titre',
                'attr' => ['placeholder' => 'Entrez le titre du livre'],
            ])
            ->add('genre', ChoiceType::class, [
                'label' => 'Genre',
                'choices' => [
                    'Roman' => 'roman',
                    'Science-fiction' => 'science_fiction',
                    'Biographie' => 'biographie',
                    'Poésie' => 'poesie',
                    'Autre' => 'autre',
                ],
                'placeholder' => 'Sélectionnez un genre',
            ])
            ->add('auteur', TextType::class, [
                'label' => 'Auteur',
                'mapped' => true,
                'attr' => ['placeholder' => 'Nom de l\'auteur'],
            ])
            ->add('submit', SubmitType::class, [
                'label' => 'Enregistrer',
            ]);
    }
}
```
## C - 2

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use App\Entity\Livre;
use App\Form\LivreType;
use Doctrine\ORM\EntityManagerInterface;

class LivreController extends AbstractController
{
	#[Route("/update-livre/{id}", name: "updateLivreRoute")]
    public function updateLivreAction(Request $request, String $id, LivreRepository $repository, EntityManagerInterface $entityManager): Response
    {
	    // Récupération du livre à modifier
        $livre = $repository->find($id);
		// Creation du formulaire
        $form = $this->createForm(LivreType::class, $livre);
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
	        // Update Livre
	        $entityManager->persist($livre);
            $entityManager->flush();
            return $this->redirectToRoute('listeLivreRoute');
        }

        return $this->render(
            'Templates/Livre/updateLivre.html.twig',
            [
                'form' => $form->createView(),
                'livre' => $livre,
            ]
        );
    }
}
```
## C - 3

```twig
<h1>Mise à jour d'un livre</h1>
<form action="{{ path('updateLivreRoute/{{livre.id}}') }}" method="post">
    {{ form_start(form) }}
    {{ form_widget(form) }}
    <button type="submit">Mettre à jour</button>
    {{ form_end(form) }}
</form>
```

## D - 1

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\RedirectResponse;
use Doctrine\ORM\EntityManagerInterface;
use App\Repository\AuteurRepository;
use Symfony\Component\Routing\Annotation\Route;

class AuteurController extends AbstractController
{
    /**
     * @Route("/auteur/delete/{id}", name="deleteAuteurRoute")
     */
    #[Route("/auteur/delete/{id}", name="deleteAuteurRoute")]
    public function deleteAuteurAction(Request $request,String $id, AuteurRepository $auteurRepository, EntityManagerInterface $entityManager): RedirectResponse
    {
        $auteur = $auteurRepository->find($id);

        try {
            $entityManager->remove($auteur);
            $entityManager->flush();

            $this->addFlash('success', 'Auteur supprimé avec succès');
        } catch (\Exception $e) {
            $this->addFlash('error', 'Erreur lors de la suppression de l\'auteur');
        }

        return $this->redirectToRoute('ListeAuteurRoute');
    }
}
```