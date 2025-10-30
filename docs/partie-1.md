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


## #[ApiResource]

### Propriétés virtuelles
Dès qu'on applique l'attribut `#[ApiResource]` sur une entité doctrine, on a automatiquement une ressource Api Platform avec le CRUD complet dans la doc `/api`.

Par défaut, ça retournera toutes les propriétés publiques et tous les getters, donc on peut créer des `propriétés virtuelles` très facilement en faisant :

```php
public function getTrucBidule(): string
{
    return 'truc bidule';
}
```

Et ça générera automatiquement ce schéma de réponse :

![fieldTrucBidule.png](images/partie-1/02-fieldTrucBidule.png)

### Groupes de serialization

Par défaut, tous les champs sont exposés mais souvent il y a certains champs ou certaines méthodes qu'on ne souhaite pas exposer, pour ce cas de figure on peut utiliser les `groupes de serialization` (de Symfony):

![05-champs-avec-groupes.png](images/partie-1/05-champs-avec-groupes.png)

Avec la config suivante dans l'attribut `#[ApiResource]` pour utiliser les groupes :

![03-groupes-de-serialization.png](images/partie-1/03-groupes-de-serialization.png)

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



| Colonne 1 | Colonne 2 |
|-----------|-----------|
| Donnée A  | Donnée B  |

- [x] Tâche complétée
- [ ] Tâche à faire

> ⚠️ **Attention**
>
> Contenu important à ne pas manquer

> 💡 **Conseil**
>
> Astuce utile pour l'utilisateur

> ℹ️ **Information**
>
> Détails supplémentaires



Un saut  
de ligne

~~Texte barré ?~~

==highlight ?==
