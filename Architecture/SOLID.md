# Principios SOLID en Python

> Fundamentos para código mantenible y extensible — Robert C. Martin ("Uncle Bob")

---

## S — Single Responsibility Principle
### *Una clase, una razón para cambiar*

Cada clase o función debe tener **una sola responsabilidad**. Si una clase hace demasiadas cosas, un cambio en una parte puede romper otra.

### ❌ Ejemplo malo

```python
# MAL: Una clase hace todo
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_user_info(self):
        return f"{self.name} - {self.email}"

    def save_to_database(self):
        # Lógica de base de datos aquí
        print(f"Guardando {self.name} en DB...")

    def send_welcome_email(self):
        # Lógica de email aquí
        print(f"Enviando email a {self.email}...")
```

### ✅ Ejemplo bueno

```python
# BIEN: Cada clase tiene una responsabilidad
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_user_info(self):
        return f"{self.name} - {self.email}"


class UserRepository:
    def save(self, user: User):
        print(f"Guardando {user.name} en DB...")


class EmailService:
    def send_welcome(self, user: User):
        print(f"Enviando email a {user.email}...")
```

---

## O — Open/Closed Principle
### *Abierto para extensión, cerrado para modificación*

El código debe poder extenderse con nuevo comportamiento **sin modificar el código existente**. Se logra mediante abstracciones e interfaces.

### ❌ Ejemplo malo

```python
# MAL: Modificar la clase para cada nuevo tipo
class AreaCalculator:
    def calculate(self, shape):
        if shape['type'] == 'circle':
            return 3.14 * shape['radius'] ** 2
        elif shape['type'] == 'rectangle':
            return shape['width'] * shape['height']
        # Hay que modificar aquí cada vez que
        # se añade una nueva forma
```

### ✅ Ejemplo bueno

```python
# BIEN: Extender sin modificar
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float:
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self) -> float:
        return 3.14 * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, w, h):
        self.width, self.height = w, h

    def area(self) -> float:
        return self.width * self.height

# Añadir Triangle no modifica nada existente
class Triangle(Shape):
    def __init__(self, base, height):
        self.base, self.height = base, height

    def area(self) -> float:
        return 0.5 * self.base * self.height
```

---

## L — Liskov Substitution Principle
### *Las subclases deben ser sustituibles por sus padres*

Si `S` es subtipo de `T`, los objetos de tipo `T` pueden reemplazarse por objetos de tipo `S` sin alterar el comportamiento del programa.

### ❌ Ejemplo malo

```python
# MAL: La subclase rompe el contrato del padre
class Bird:
    def fly(self):
        return "Estoy volando!"

class Penguin(Bird):
    def fly(self):
        # Los pingüinos no vuelan!
        # Esto viola Liskov
        raise Exception("No puedo volar!")

def make_bird_fly(bird: Bird):
    # Esto falla con Penguin
    return bird.fly()
```

### ✅ Ejemplo bueno

```python
# BIEN: Jerarquía correcta con abstracciones
from abc import ABC, abstractmethod

class Bird(ABC):
    @abstractmethod
    def move(self) -> str:
        pass

class FlyingBird(Bird):
    def fly(self) -> str:
        return "Estoy volando!"

    def move(self) -> str:
        return self.fly()

class Eagle(FlyingBird):
    pass  # Hereda fly() correctamente

class Penguin(Bird):
    def swim(self) -> str:
        return "Estoy nadando!"

    def move(self) -> str:
        return self.swim()

# Ambos funcionan polimórficamente
for bird in [Eagle(), Penguin()]:
    print(bird.move())
```

---

## I — Interface Segregation Principle
### *Muchas interfaces específicas mejor que una general*

Los clientes no deben depender de interfaces que no usan. Es mejor tener interfaces pequeñas y específicas que una grande y genérica.

### ❌ Ejemplo malo

```python
# MAL: Interfaz gorda que obliga implementar todo
from abc import ABC, abstractmethod

class Worker(ABC):
    @abstractmethod
    def work(self): pass

    @abstractmethod
    def eat(self): pass

    @abstractmethod
    def sleep(self): pass

class Robot(Worker):
    def work(self):
        return "Trabajando..."

    def eat(self):
        # Los robots no comen!
        raise NotImplementedError

    def sleep(self):
        # Los robots no duermen!
        raise NotImplementedError
```

### ✅ Ejemplo bueno

```python
# BIEN: Interfaces pequeñas y específicas
from abc import ABC, abstractmethod

class Workable(ABC):
    @abstractmethod
    def work(self): pass

class Eatable(ABC):
    @abstractmethod
    def eat(self): pass

class Sleepable(ABC):
    @abstractmethod
    def sleep(self): pass

class Human(Workable, Eatable, Sleepable):
    def work(self):  return "Trabajando..."
    def eat(self):   return "Comiendo..."
    def sleep(self): return "Durmiendo..."

class Robot(Workable):
    # Solo implementa lo que necesita
    def work(self): return "Trabajando 24/7..."
```

---

## D — Dependency Inversion Principle
### *Depende de abstracciones, no de implementaciones*

Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones. Esto facilita el testing y la flexibilidad.

### ❌ Ejemplo malo

```python
# MAL: Dependencia directa de implementación
class MySQLDatabase:
    def save(self, data):
        print(f"Guardando en MySQL: {data}")

class UserService:
    def __init__(self):
        # Acoplado directamente a MySQL!
        self.db = MySQLDatabase()

    def create_user(self, name):
        self.db.save(name)

# Imposible cambiar la DB sin modificar UserService
```

### ✅ Ejemplo bueno

```python
# BIEN: Depender de la abstracción
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def save(self, data): pass

class MySQLDatabase(Database):
    def save(self, data):
        print(f"Guardando en MySQL: {data}")

class MongoDatabase(Database):
    def save(self, data):
        print(f"Guardando en MongoDB: {data}")

class UserService:
    def __init__(self, db: Database):
        # Depende de la abstracción
        self.db = db

    def create_user(self, name):
        self.db.save(name)

# Fácil de cambiar o testear
service = UserService(MySQLDatabase())
service = UserService(MongoDatabase())
```

---

## Resumen rápido

| Letra | Principio | Idea clave |
|-------|-----------|------------|
| **S** | Single Responsibility | Una clase, una razón para cambiar |
| **O** | Open/Closed | Extender sin modificar |
| **L** | Liskov Substitution | Las subclases respetan el contrato del padre |
| **I** | Interface Segregation | Interfaces pequeñas y específicas |
| **D** | Dependency Inversion | Depender de abstracciones, no de concretos |



# Littio
Banking Core (Cerebro de Littio) Enfocado a backend. Clientes son clientes internos de la compañia. 

- Hay un buen ritmo
- 

Equipos oreintados al consumidor final. Meter oro, wallet, USD, Euro. Plataforma son muy buenos. Datos.

