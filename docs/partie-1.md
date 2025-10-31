# Part 1: Mythically Good RESTful APIs


## Table des matières :
- [Lexique](#Lexique)
- [#[ApiResource]](#ApiResource)
- - [Propriétés virtuelles](#Propriétés-virtuelles)
- - [Groupes de serialization](#Groupes-de-serialization)
- - [Pagination](#Pagination)
- [Filters](#Filters)


## Lexique

**OpenAPI (anciennement Swagger Specification)**
> C'est un standard de description d'API REST dans un format JSON ou YAML.

**Swagger**
> C'était le nom original, c'est aujourd'hui une suite d'outils autour d'OpenAPI.

**Swagger UI**
> C'est une interface web interactive générée à partir da la config OpenAPI.

**RDF (Resource Description Framework)**
> C'est un standard du W3C conçu pour décrire des ressources et leurs relations.


**JSON-LD (JSON for Linked Data)**
> JSON-LD est un standard du W3C conçu pour représenter des données RDF.

**Hydra**
> Hydra est un vocabulaire RDF du W3C conçu pour décrire les API pour les machines.

**IRI (Internationalized Resource Identifier)**
> C'est une adresse qui désigne une ressource unique au format URI.

**URI (Uniform Resource Identifier)**
> Contrairement aux URL qui n'acceptent que l'ascii (a-z), tous les caractères unicode sont acceptés.

## #[ApiResource]

Dès qu'on applique l'attribut `#[ApiResource]` sur une entité doctrine, on a automatiquement une ressource Api Platform avec le CRUD complet dans la doc `/api`.

### Propriétés virtuelles

Par défaut, ça retournera toutes les propriétés publiques et tous les getters, donc on peut créer des `propriétés virtuelles` très facilement en faisant :

```php
public function getTrucBidule(): string
{
    return 'truc bidule';
}
```

Et ça générera automatiquement ce schéma de réponse avec le champs `trucBidule` :

```json
{
  "@id": "/api/treasures/1",
  "@type": "Treasure",
  "id": 1,
  "owner": "/api/users/3",
  "name": "set of ornate jewelry",
  "shortDescription": "Fugit quo perspiciatis est ratione do...",
  "plunderedAtAgo": "2 months ago",
  "trucBidule": "truc bidule"
}
```

### Groupes de serialization

Par défaut, tous les champs sont exposés mais souvent il y a certains champs ou certaines méthodes qu'on ne souhaite pas exposer, pour ce cas de figure on peut utiliser les `groupes de serialization` (de Symfony):

```php
#[ORM\Column(length: 255)]
#[Groups(['treasure:read', 'treasure:write'])]
private ?string $name = null;

#[ORM\Column(type: Types::TEXT)]
#[Groups('treasure:read')]
private ?string $description = null;
```


Avec la config suivante dans l'attribut `#[ApiResource]` pour utiliser les groupes :

```php
#[ApiResource(
    shortName: 'Treasure',
    description: 'A rare and valuable treasure.',
    normalizationContext: ['groups' => ['treasure:read']],
    denormalizationContext: ['groups' => ['treasure:write']],
)]
```

Pour rappel, voici à quoi correspondent la `normalisation` et la `denormalisation` :
![04-schema-serialization.png](images/partie-1/04-schema-serialization.png)

> ℹ️ **Information**
> 
> On peut renommer les champs exposés avec l'attribut `#[SerializedName]`


### Pagination

L'attribut `#[ApiResource]` permet de configurer la pagination pour chaque ressource avec certaines options comme :

```php
#[ApiResource(
    paginationItemsPerPage: 10,
    paginationMaximumItemsPerPage: 100
)]
```


## Filters

Les filtres peuvent s'appliquer avec l'attribut `#[ApiFilter(MonFiltre::class)]` qu'on peut mettre soit sur la classe avec potentiellement l'option `properties` soit directement sur la bonne propriété.
> ⚠️ **Attention**
>
> L'attribut `#[ApiFilter]` a été déprécié et sera supprimé dans Api Platform 5 !  
> On doit préférer l'attribut `#[QueryParameter]` qui lui ne s'applique sur sur la classe.


La liste des filtres disponibles :

`BooleanFilter`
: Permet de filtrer sur un booléen (`?property=<true|false|1|0>`)

`DateFilter`
: Permet de filtrer sur une date (`?property[<after|before|strictly_after|strictly_before>]=value`)

`ExistsFilter`
: Permet de filtrer les valeurs nulles (`?property=<true|false|1|0>`)

`NumericFilter`
: Permet de filtrer sur un nombre exact (`?property=<int|bigint|decimal...>`)

`RangeFilter`
: Permet de filtrer sur une comparaison numérique (`?property[<lt|gt|lte|gte|between>]=value`)

`OrderFilter`
: Ne permet pas de filtrer mais de **trier** (`?order[property]=<asc|desc>`)

### Search Filter

Il y avait historiquement le `SearchFilter` qui pouvait recevoir une `strategy` en option.  
Les stratégies étaient :`exact`, `partial`, `start`, `end`, et `word_start`.  
Avec la possibilité de prefixer la stratégie par un `i` pour rendre la recherche `case insensitive`.

Cependant, `SearchFilter` **a été déprécié**  et remplacé par 3 nouveaux filtres :

`ExactFilter`
: Permet de filtrer sur une valeur exacte (`?property=value`)

`IriFilter`
: Permet de filtrer sur un IRI (`?property=value`)

`PartialSearchFilter`
: Permet de filtrer sur une correspondance partielle et **case insensitive** (`?property=value`)


### Nouveaux filtres spéciaux

Il y a eu 2 nouveaux filtres qui permettent de combiner plusieurs filtres :

`FreeTextQueryFilter`
: Prend en argument un filtre pour pouvoir l'appliquer sur plusieurs propriétés à la fois (`?property=value`)


`OrFilter`
: Prend en argument un filtre pour permettre d'avoir un filtre qui match mais pas nécessairement tous.

### Examples


| Colonne 1 | Colonne 2 |
|-----------|-----------|
| Donnée A  | Donnée B  |

- [x] Tâche complétée
- [ ] Tâche à faire



> 💡 **Conseil**
>
> Astuce utile pour l'utilisateur

> ℹ️ **Information**
>
> Détails supplémentaires



Un saut  
de ligne

~~Texte barré ?~~

