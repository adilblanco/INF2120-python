# Orienté objet

## 1. Héritage

**Héritage**
+ vers le bas: (&darr;) spécialisation
+ vers le haut: (&uarr;) généralisation

**Intérêt de l'héritage**
+ réutiliser ce qui existe déja
+ sauver du temps
+ éviter la répétition de code
+ favoriser la maintenace
+ regrouper les caractéristiques communes

<mark>
Lorsqu’une classe n’hérite pas explicitement d’une autre classe, celle-ci hérite directement de la classe Object.
</mark>

```python
class User(Object):
    pass
```
<mark>
L’instruction `pass` sert ici à indiquer à Python que le corps de notre classe est vide
</mark>

**Une classe dérivée est responsable de la construction de sa classe de base**: pour pouvoir construire un `Etudiant`, il faut d’abord construire une `Personne`.

```python
class Personne:
    def __init__(self, nom: str, age: int) -> None:
        self.nom = nom
        self.age = age

    def __str__(self) -> str:
        return f"{self.__class__.__name__}: {self.nom}, {self.age}"
```
```python
class Etudiant(Personne):
    def __init__(self, nom: str, age: int, code: str) -> None:
        # Personne.__init__(self, nom, age)     
        super().__init__(nom, age)
        self.code = code

    def __str__(self) -> str:
        return f"{super().__str__()}, {self.code}"
```
```python
print(Personne("Joe", 28))
print(Etudiant("Doe", 20, "JODO11128803"))

>> Personne: Joe, 28
>> Etudiant: Doe, 20, JODO11128803
```

## 2. Encapsulation

La visibilité des membres d’une classe (attributs et méthodes) peut être déterminée à l’aide de 3 modificateurs d’accès:

### public
+ Chaque attribut et méthode que nous avons utilisé jusqu'à présent est public
+ Les attributs et les méthodes peuvent être modifiés et exécutés de partout à l'intérieur ou à l'extérieur de la classe.

### protected
+ Les attributs et les méthodes sont accessibles depuis la classe et les sous-classes
+ Attributs et méthodes précédés d'un trait de soulignement _

### private
+ Les attributs et les méthodes sont accessibles depuis la classe ou l'objet uniquement
+ Les attributs ne peuvent pas être modifiés depuis l'extérieur de la classe
+ Attributs et méthodes précédés de deux traits de soulignement __

<mark>
Python n'a aucun mécanisme qui empêche d'accéder à une variable ou appeler une méthode membre. Tout cela est une question de culture et de convention. Toutes les variables et méthodes membres sont publiques par défaut.
</mark>

## 3. Surdéfinition et redéfinition

### 5.1 Signature d’une méthode
La signature d’une méthode (ou d’un constructeur) comprend:
+ Nom de la méthode (ou du constructeur)
+ `Nombre de paramètres` et leur `type`, dans l’`ordre` où ils apparaissent (le nom des
paramètres n’est pas considéré).

**Note**:
+ Le type de retour (pour les méthodes) ne fait pas partie de la signature de la méthode.
+ À l’intérieur d’une même classe, les méthodes (ou constructeurs) doivent posséder des signatures différentes.

### 5.2 Surdéfinition (ou surcharge) de méthodes
+ La surdéfinition consiste à définir des méthodes ayant le même nom, mais de signatures différentes.
+ Une sous-classe peut bien sûr surcharger une méthode de sa superclasse.

### 5.3 Redéfinition (ou masquage) de méthodes
Une sous-classe peut redéfinir (ou masquer) des méthodes de sa classe de base.


La redéfinition d’une méthode dans une sous-classe doit obéir aux principes suivants :
+ La méthode redéfinie doit avoir une signature identique à la méthode de sa superclasse.
+ La méthode redéfinie doit avoir le même type de retour que celle de sa classe mère.
+ La méthode redéfinie doit avoir une visibilité égale ou supérieure à celle de sa classe de base. La redéfinition ne doit pas diminuer les droits d’accès.

```python
class A:
    def machin(x: int) -> None: pass            #nouvelle définition
    def machin(x: int, y: int) -> float: pass   #surdéfinition
    def truc() -> int: pass                     #nouvelle définition
    def __str__() -> str: pass                  #redéfinition (__str__ de la class object)


class B(A):
    def machin(s: str) -> None: pass            #surdéfinition
    def truc() -> int: pass                     #redéfinition
    def chose() -> None: pass                   #nouvelle définition
```

## 4. Polymorphisme

Une entité polymorphe est une entité qui peut prendre plusieurs formes. Le polymorphisme est la propriété de ce qui est polymorphe.

Une méthode exécutée sur un objet est choisie en fonction du type dynamique de l’objet sur lequel elle s’applique, au moment de l’exécution.

le polymorphisme est donc mis en oeuvre par l’intermédiaire de l’`héritage` et de la `redéfinition` de méthodes. Ainsi, lorsqu’on appelle une méthode redéfinie dans plusieurs classes d’une hiérarchie, celle qui sera effectivement exécutée sera choisie en fonction du type de l’objet au moment de l’exécution (type dynamique).
