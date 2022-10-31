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

```python
import crypt

class User:
    def __init__(self, id, name, password):
        self.id = id
        self.name = name
        self._salt = crypt.mksalt()
        self._password = self._crypt_pwd(password) 

    def __str__(self) -> str:
        return f"{self.id}, {self.name}, {self._salt}, {self._password}"
        
    def _crypt_pwd(self, password):
        return crypt.crypt(password, self._salt)

    def check_pwd(self, password):
        return self._password == self._crypt_pwd(password)


john = User(1, 'John', '12345')
print(john)
print(john._password)
print(john.check_pwd('12345'))
```
```
>> 1, John, Jg, JggqoyxYzTiqQ
>> JggqoyxYzTiqQ
>> True
```

On note toutefois qu’il ne s’agit que d’une convention, l’attribut _password étant parfaitement visible depuis l’extérieur.

Il reste possible de masquer un peu plus l’attribut à l’aide du préfixe __. Ce préfixe a pour effet de renommer l’attribut en y insérant le nom de la classe courante.

```python
import crypt

class User:
    def __init__(self, id, name, password):
        self.id = id
        self.name = name
        self.__salt = crypt.mksalt()
        self.__password = self.__crypt_pwd(password) 

    def __str__(self) -> str:
        return f"{self.id}, {self.name}, {self.__salt}, {self.__password}"
        
    def __crypt_pwd(self, password):
        return crypt.crypt(password, self.__salt)

    def check_pwd(self, password):
        return self.__password == self.__crypt_pwd(password)


john = User(1, 'John', '12345')
print(john)
print(john.__password)
```
```
>> 1, John, eG, eGK7PvBmZngRY
>>  Traceback (most recent call last):
        File "main.py", line 22, in <module>
        print(john.__password)
    AttributeError: 'User' object has no attribute '__password'
```

### 3.3 Duck Typing

Un objet en Python est défini par sa structure (les attributs qu’il contient et les méthodes qui lui sont applicables) plutôt que par son type.

Duck Typing est un terme couramment lié aux langages de programmation à `typage dynamique` et au `polymorphisme`. L'idée derrière ce principe est que le code lui-même ne se soucie pas de savoir si un objet est un `duck`, mais à la place, il ne se soucie que de savoir s'il `quacks`.

> If it walks like a duck, and it quacks like a duck, then it must be a duck.

```
>> a = 10
>> b = 12
>> a + b
>> 22
```

```
>> a = '10'
>> b = '12'
>> a + b
>> 1012
```

Ce comportement `polymorphe` est une idée centrale derrière Python, qui est également un `langage à typage dynamique`. Cela signifie qu'il effectue une vérification de `type` au moment de l'exécution, contrairement aux langages à typage statique (tels que Java) qui l'effectuent au moment de la compilation.

## 4. Héritage

```python
class User:
    def __init__(self, id, name, password):
            self.id = id
            self.name = name
            self._salt = crypt.mksalt()
            self._password = self._crypt_pwd(password)

    def _crypt_pwd(self, password):
        return crypt.crypt(password, self._salt)
    
    def check_pwd(self, password):
        return self._password == self._crypt_pwd(password)


class Admin(User):
    def manage(self):
        print("Je suis uber administrateur!")


class Guest(User): 
    pass


class SuperAdmin(Admin): 
    pass
```

### 4.1. Sous-typage

En Python, la fonction isinstance permet de tester si un objet est l’instance d’une certaine classe.

```
>> root = Admin(1, "root", "toor")

>>> isinstance(root, Admin)
True
>>> isinstance(root, User)
True
>>> isinstance(root, Guest)
False
>>> isinstance(root, object)
True
```
### 4.2. Redéfinition

```python
class Guest(User):
def __init__(self, id, name):
        self.id = id
        self.name = name
        self._salt = ''
        self._password = ''

def check_pwd(self, password): 
    return True
```

### 4.3. Super

```python
class Guest(User): 
    def __init__(self, id, name):
         super().__init__(id, name, '')

    def check_pwd(self, password): 
        return True
```

### 4.4. Double héritage

```python
class A:
    def foo(self):
        return '!'

class B:
    def bar(self):
        return '?'

class C(A, B): 
    pass
```

```
>> C.mro()
>> [<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>]
```

C’est aussi ce `MRO` qui est utilisé par `super` pour trouver à quelle classe faire appel.

### 4.5. Mixins

Les `mixins` sont des classes dédiées à une fonctionnalité particulière, utilisable en héritant d’une classe de base et de ce mixin.

```python
class Reversable:
    def reverse(self):
        return self[::-1]

class ReversableStr(Reversable, str): 
    pass

class ReversableTuple(Reversable, tuple): 
    pass

class ReversableList(Reversable, list): 
    pass
```
```
>> print(ReversableList([2, 4, 6]).reverse())
>> [6, 4, 2]

>> print(ReversableStr('abc').reverse())
>> cba

>> print(ReversableTuple((1, 2, 3)).reverse())
>> (3, 2, 1)
```