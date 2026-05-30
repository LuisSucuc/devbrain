
# Patterns of Design

---

## Chain of Responsibility

Patrón de comportamiento que permite pasar una solicitud a lo largo de una cadena de manejadores. Cada manejador decide si procesa la solicitud o la pasa al siguiente en la cadena. El emisor no sabe cuál manejador la resolverá.

> Se usa en los middlewares de Express: cada middleware recibe la request, hace algo (validar, autenticar, loggear) y decide si llama a `next()` para pasarla al siguiente o corta la cadena respondiendo directamente.

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class Handler(ABC):
    _next: Handler | None = None

    def set_next(self, handler: Handler) -> Handler:
        self._next = handler
        return handler

    @abstractmethod
    def handle(self, request: str) -> str | None:
        if self._next:
            return self._next.handle(request)
        return None


class AuthHandler(Handler):
    def handle(self, request: str) -> str | None:
        if request == "no-auth":
            return "AuthHandler: acceso denegado."
        return super().handle(request)


class LogHandler(Handler):
    def handle(self, request: str) -> str | None:
        print(f"LogHandler: registrando request '{request}'")
        return super().handle(request)


class BusinessHandler(Handler):
    def handle(self, request: str) -> str | None:
        return f"BusinessHandler: procesando '{request}'"


# Construcción de la cadena
auth = AuthHandler()
log = LogHandler()
business = BusinessHandler()

auth.set_next(log).set_next(business)

print(auth.handle("no-auth"))   # corta en AuthHandler
print(auth.handle("get-data"))  # pasa por toda la cadena
```

---

## Composite

Patrón estructural que permite componer objetos en estructuras de árbol para representar jerarquías parte-todo. Tanto los objetos individuales (hojas) como los compuestos (nodos) se tratan de manera uniforme a través de una interfaz común.

> En videojuegos, si tienes un jefe con minions: el jefe es un nodo compuesto y los minions son hojas. Al llamar `die()` en el jefe, la operación se propaga a todos sus hijos automáticamente.

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class Enemy(ABC):
    @abstractmethod
    def die(self) -> None:
        ...


class Minion(Enemy):
    def __init__(self, name: str) -> None:
        self.name = name

    def die(self) -> None:
        print(f"Minion {self.name} ha muerto.")


class Boss(Enemy):
    def __init__(self, name: str) -> None:
        self.name = name
        self._children: list[Enemy] = []

    def add(self, enemy: Enemy) -> None:
        self._children.append(enemy)

    def die(self) -> None:
        print(f"Boss {self.name} ha muerto. Sus hijos también caen:")
        for child in self._children:
            child.die()


goblin = Minion("Goblin")
orc = Minion("Orc")
dragon = Boss("Dragon")
dragon.add(goblin)
dragon.add(orc)

dragon.die()
```

---

## Prototype

Patrón de creación que permite copiar objetos existentes sin depender de sus clases concretas. En lugar de instanciar desde cero, se clona un objeto prototipo y se ajustan solo las propiedades necesarias.

> Útil cuando la creación de un objeto es costosa (conexiones, cálculos) y se necesitan múltiples instancias similares. Se clona el prototipo y se modifican solo los campos que cambian.

```python
import copy
from dataclasses import dataclass, field


@dataclass
class EnemyConfig:
    name: str
    health: int
    abilities: list[str] = field(default_factory=list)

    def clone(self) -> "EnemyConfig":
        return copy.deepcopy(self)


base_goblin = EnemyConfig(name="Goblin", health=100, abilities=["attack", "dodge"])

elite_goblin = base_goblin.clone()
elite_goblin.name = "Elite Goblin"
elite_goblin.health = 250
elite_goblin.abilities.append("magic")

print(base_goblin)   # Goblin, 100hp, ['attack', 'dodge']
print(elite_goblin)  # Elite Goblin, 250hp, ['attack', 'dodge', 'magic']
```

---

## State

Patrón de comportamiento que permite a un objeto alterar su comportamiento cuando su estado interno cambia. El objeto parecerá cambiar de clase porque delega su lógica al objeto de estado actual.

> Un semáforo cambia de comportamiento (qué acción permite) dependiendo de si está en rojo, amarillo o verde. En lugar de llenar el código de `if/else`, cada estado encapsula su propia lógica.

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class TrafficLightState(ABC):
    @abstractmethod
    def next_state(self, light: TrafficLight) -> None:
        ...

    @abstractmethod
    def action(self) -> str:
        ...


class RedState(TrafficLightState):
    def action(self) -> str:
        return "Detente."

    def next_state(self, light: TrafficLight) -> None:
        light.state = GreenState()


class GreenState(TrafficLightState):
    def action(self) -> str:
        return "Avanza."

    def next_state(self, light: TrafficLight) -> None:
        light.state = YellowState()


class YellowState(TrafficLightState):
    def action(self) -> str:
        return "Precaución."

    def next_state(self, light: TrafficLight) -> None:
        light.state = RedState()


class TrafficLight:
    def __init__(self) -> None:
        self.state: TrafficLightState = RedState()

    def change(self) -> None:
        print(self.state.action())
        self.state.next_state(self)


light = TrafficLight()
for _ in range(6):
    light.change()
```

---

## Flyweight

Patrón estructural que minimiza el uso de memoria compartiendo la mayor cantidad posible de datos entre objetos similares. Separa el estado **intrínseco** (compartido, inmutable) del estado **extrínseco** (único por instancia).

> En un videojuego con miles de partículas, la textura y el color son iguales para todas las partículas del mismo tipo (estado intrínseco, se guarda una sola vez en RAM). La posición y velocidad son únicas para cada partícula (estado extrínseco, se pasa en cada operación). Sin Flyweight, cada partícula cargaría su propia copia de la textura.

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class ParticleTexture:
    """Estado intrínseco: compartido entre todas las partículas del mismo tipo."""
    color: str
    sprite: str


class ParticleFactory:
    _cache: dict[tuple, ParticleTexture] = {}

    @classmethod
    def get(cls, color: str, sprite: str) -> ParticleTexture:
        key = (color, sprite)
        if key not in cls._cache:
            cls._cache[key] = ParticleTexture(color=color, sprite=sprite)
            print(f"Creando nueva textura: {key}")
        return cls._cache[key]


@dataclass
class Particle:
    x: float
    y: float
    texture: ParticleTexture  # referencia compartida, no copia


particles = [
    Particle(x=i * 0.1, y=i * 0.2, texture=ParticleFactory.get("red", "fire.png"))
    for i in range(1000)
]

print(f"Partículas: {len(particles)}")
print(f"Texturas en memoria: {len(ParticleFactory._cache)}")  # solo 1
```
