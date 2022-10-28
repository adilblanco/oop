# 1. Introduction
+ OOP est un paradigme de programmation; c’est-à-dire une façon de concevoir un programme informatique.
+ OOP nous permet de définir nos propres types d’objets, avec leurs propriétés et opérations.

# 2. Objet et caractéristiques
Un objet est constitué de 3 caractéristiques:
+ Type
+ attributs
+ methodes

```
>>> number = 5                  # On instancie une variable `number` de type `int`
>>> number.numerator            # `numerator` est un attribut de `number`
>>> 5
>>> values = []                 # Variable `values` de type `list`
>>> values.append(number)       # `append` est une méthode de `values`
>>> [5]
```

## 2.1. Il a une drôle de tête ce type-là

```
class User:
    pass
```

## 2.2. Montre-moi tes attributs
```
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


## 2.3. Discours de la méthode
```
def user_check_pwd(user, password):
    return user.password == password

>>> user_check_pwd(john, 'toto')
False
>>> user_check_pwd(john, '12345')
True
```

# 3. Classes
```
class User:
    pass
```

L’instruction pass sert ici à indiquer à Python que le corps de notre classe est vide

```
>>> User()
<__main__.User object at 0x7fc28e538198>
>>> int()
0
>>> str()
''
>>> list()
[]
```

## 3.2. Argumentons pour construire
Nous avons vu qu’instancier une classe était semblable à un appel de fonction. Dans ce cas,
comment passer des arguments à une classe

```
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

```
>>> john = User(1, 'john', '12345')
>>> john.check_pwd('toto')
False
>>> john.check_pwd('12345')
True
```

## 3.3. Comment veux-tu que je t’encapsule ?

```
import crypt

class User:
    def __init__(self, id, name, password):
        self.id = id
        self.name = name
        self._salt = crypt.mksalt()     # sel utilisé pour le hash du mot de passe
        self._password = self._crypt_pwd(password)


    def _crypt_pwd(self, password):
        return crypt.crypt(password, self._salt)

    def check_pwd(self, password):
        return self._password == self._crypt_pwd(password)

    >>> john = User(1, 'john', '12345')
    >>> john.check_pwd('12345')
    True
```

On note toutefois qu’il ne s’agit que d’une convention, l’attribut _password étant parfaitement
visible depuis l’extérieur.

```
>>> john._password
'$6$DwdvE5H8sT71Huf/$9a.H/VIK4fdwIFdLJYL34yml/QC3KZ7'
```

> 1. Le hashage d’un mot de passe correspond à une opération non-réversible qui permet de calculer un
> condensat (hash) du mot de passe. Ce condensat peut-être utilisé pour vérifier la validité d’un mot de passe,
> mais ne permet pas de retrouver le mot de passe d’origine.

Il reste possible de masquer un peu plus l’attribut à l’aide du préfixe __ . Ce préfixe a pour effet
de renommer l’attribut en y insérant le nom de la classe courante.

```
class User:
    def __init__(self, id, name, password):
        self.id = id
        self.name = name
        self.__salt = crypt.mksalt()
        self.__password = self.__crypt_pwd(password)

    def __crypt_pwd(self, password):
        return crypt.crypt(password, self.__salt)

    def check_pwd(self, password):
        return self.__password == self.__crypt_pwd(password)
```

```
>>> john = User(1, 'john', '12345')
>>> john.__password
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
AttributeError: 'User' object has no attribute '__password'
>>> john._User__password
'$6$kjwoqPPHRQAamRHT$591frrNfNNb3.RdLXYiB/bgdCC4Z0p.B'
```

Ce comportement pourra surtout être utile pour éviter des conflits de noms entre attributs
internes de plusieurs classes sur un même objet, que nous verrons lors de l’héritage.
