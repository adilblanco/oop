# Python OOP

## 1. Introduction

+ OOP est un paradigme de programmation; c’est-à-dire une façon de concevoir un programme informatique.
+ OOP nous permet de définir nos propres types d’objets, avec leurs propriétés et opérations.

## 2. Objet et caractéristiques

Un objet est constitué de 3 caractéristiques:

+ Types
+ attributs
+ methodes

```python
>>> number = 5                  # On instancie une variable `number` de type `int`
>>> number.numerator            # `numerator` est un attribut de `number`
>>> 5
>>> values = []                 # Variable `values` de type `list`
>>> values.append(number)       # `append` est une méthode de `values`
>>> [5]
```

### 2.1. Types

```python
class User:
    pass
```

### 2.2. Attributs

```python
class User:
    pass

# Instanciation d'un objet de type User
john = User()

# Définition d'attributs pour cet objet
john.id = 1
john.name = 'john'
john.password = '12345'

print('Bonjour, je suis {}.'.format(john.name))
print('Mon id est le {}.'.format(john.id))
print('Mon mot de passe est {}.'.format(john.password))
```

Nous avons instancié un objet nommé john , de type User , auquel nous avons attribué trois
attributs. Puis nous avons aﬃché les valeurs de ces attributs.

### 2.3. Méthodes

```python
def user_check_pwd(user, password):
    return user.password == password

>>> user_check_pwd(john, 'toto')
False
>>> user_check_pwd(john, '12345')
True
```

## 3. Classes

```python
class User:
    pass
```

L’instruction `pass` sert ici à indiquer à Python que le corps de notre classe est vide

```python
>>> User()
<__main__.User object at 0x7fc28e538198>
>>> int()
0
>>> str()
''
>>> list()
[]
```

### 3.1. constructeur

Nous avons vu qu’instancier une classe était semblable à un appel de fonction. Dans ce cas,
comment passer des arguments à une classe

```python
class User:
def __init__(self, id, name, password):
    self.id = id
    self.name = name
    self.password = password

def check_pwd(self, password):
    return self.password == password
```

Nous retrouvons dans cette méthode le paramètre `self` , qui est donc utilisé pour modifier les
attributs de l’objet

```python
>>> john = User(1, 'john', '12345')
>>> john.check_pwd('toto')
False
>>> john.check_pwd('12345')
True
```

### 3.2. Encapsulation

Restreindre l'accès aux données stockées dans les attributs et les méthodes

**public**:
+ Chaque attribut et méthode que nous avons utilisé jusqu'à présent est public
+ Les attributs et les méthodes peuvent être modifiés et exécutés de partout à l'intérieur ou à l'extérieur de la classe.

**Portected**:
+ Les attributs et les méthodes sont accessibles depuis la classe et les sous-classes
+ Attributs et méthodes précédés d'un trait de soulignement _

**private**:
+ Les attributs et les méthodes sont accessibles depuis la classe ou l'objet uniquement
+ Les attributs ne peuvent pas être modifiés depuis l'extérieur de la classe
+ Attributs et méthodes précédés de deux traits de soulignement __

<mark>
Python n'a aucun mécanisme qui empêche d'accéder à une variable ou appeler une méthode membre. Tout cela est une question de culture et de convention. Toutes les variables et méthodes membres sont publiques par défaut.
</mark> 
