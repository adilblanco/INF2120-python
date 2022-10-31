## 1. Méthodes abstraites versus concrètes

**Méthode concrète**

Une méthode concrète est une méthode complètement définie qui possède un entête et un corps (des instructions, une implémentation).

```python
def do_something():
    return value
```

**Méthode abstraite**

Une méthode abstraite est une méthode qui ne possède pas de corps (pas d’implémentation). On déclare une méthode abstraite avec le modificateur abstract.

```python
@abc.abstractmethod
def do_something():
    raise NotImplementedError
```

## 2. Les classes abstraites

### 2.1 Notions de base

Une classe abstraite est une classe qui ne peut pas être instanciée. Une telle classe ne peut que servir de classe de base pour des classes dérivées.

Une classe abstraite :
+ **Peut contenir tout ce qu’une classe concrète** (sans le mot abstract) peut contenir (variables d’instance ou de classe, constantes d’instance ou de classe, méthodes concrètes, constructeurs... ) PLUS des méthodes abstraites.
+ **Peut hériter d’une classe** (comme une classe concrète).
+ **Peut implémenter des interfaces** (comme une classe concrète).

Lorsqu’une classe **contient** au moins une **méthode abstraite**, cette classe **doit être déclarée abstraite**.

Une classe dérivée d’une classe non abstraite peut être déclarée abstraite : une classe abstraite n’est pas toujours la classe la plus haute dans la hiérarchie.

Les méthodes abstraites d’une classe abstraite ne peuvent pas être déclarées private.

Les méthodes abstraites d’une classe abstraite **peuvent être concrétisées par ses
sous-classes**.

**Lorsqu’une sous-classe ne concrétise (n’implémente, ne redéfinit) pas toutes les méthodes abstraites**, cette sous-classe **doit alors être déclarée abstraite**.

```python
from abc import ABC, abstractmethod
 
class Polygon(ABC):
 
    @abstractmethod
    def noofsides(self):
        pass
 
class Triangle(Polygon):
 
    # overriding abstract method
    def noofsides(self):
        print("I have 3 sides")
 
class Pentagon(Polygon):
 
    # overriding abstract method
    def noofsides(self):
        print("I have 5 sides")
 
class Hexagon(Polygon):
 
    # overriding abstract method
    def noofsides(self):
        print("I have 6 sides")
 
class Quadrilateral(Polygon):
 
    # overriding abstract method
    def noofsides(self):
        print("I have 4 sides")
```
```
>> I have 3 sides
>> I have 4 sides
>> I have 5 sides
>> I have 6 sides
```

### 2.2 Intérêt des classes abstraites

On utilise une classe abstraite :

+ Lorsqu’on a besoin d’une classe pour regrouper des caractéristiques (attributs et méthodes) communes à plusieurs sous-classes.
+ L’implémentation de ces méthodes abstraits pourra varier d’une sous- classe à l’autre `Polymorphisme`.

```python
from abc import ABC, abstractmethod
 
class Animal(ABC):
 
    @abstractmethod
    def sedeplacer(self):
        pass
 
class Oiseau(Animal):
 
    def sedeplacer(self):
        print("Je vole")
 
class Chien(Animal):
 
    def sedeplacer(self):
        print("Je cours")

o = Oiseau()
o.sedeplacer()
c = Chien()
c.sedeplacer()
```
```
>> Je vole
>> Je cours
```

## 3. Les interfaces

### 3.1 Défnition d’une interface 

Une interface est un `type`, comme toute classe, et c’est un type qu’on dit `abstrait`. 

Une interface peut seulement contenir :
+ des méthodes d’instance abstraites

Une interface **ne peut donc pas** contenir :
+ de variables
+ de constructeurs
+ de méthodes concrétes

### 3.2 Implémentation d’une interface

Les méthodes d’une interface doivent être implémentées (concrétisées) par toutes les classes qui implémentent cette interface. Lorsqu’une classe implémente une interface, celle-ci (ou l’un de ces descendants) s’engage à redéfinir (concrétiser, implémenter) **toutes les méthodes de cette interface**.

```python
class IAnimal:
    def sedeplacer(): pass
    def sidentifier(): pass
    def communiquer(): pass


class Chien(IAnimal):

    def __init__(self, race, age, jappement) -> None:
        # super().__init__()
        self.race = race
        self.age = age
        self.jappement = jappement

    def sedeplacer(self):
        print("Je marche et je cours !")

    def sidentifier(self):
        print(f"Je suis un {self.race} et j'ai {self.age} ans")

    def communiquer(self):
        print(self.jappement)


class Oiseau(IAnimal):
    def __init__(self, chant, sorte, bec) -> None:
        # super().__init__()
        self.chant = chant 
        self.sorte = sorte 
        self.bec = bec 

    def sedeplacer(self):
        print("Je vole !")

    def sidentifier(self):
        print(f"Je suis un {self.sorte} et mon bec est {self.bec}")

    def communiquer(self):
        print(self.chant)

c = Chien("dogo", 8, "HouHou !")
c.sedeplacer()
c.sidentifier()
c.communiquer()

o = Oiseau("WiWi !", "perruche", "plat")
o.sedeplacer()
o.sidentifier()
o.communiquer()
```

```
>> Je marche et je cours !
>> Je suis un dogo et j'ai 8 ans
>> HouHou !

>> Je vole !
>> Je suis un perruche et mon bec est plat
>> WiWi !
```