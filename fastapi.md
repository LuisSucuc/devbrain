# Curso Completo de FastAPI

> **Nivel:** Básico → Avanzado  
> **Audiencia:** Desarrolladores de nivel middle con conocimientos previos en Python  
> **Versión de FastAPI:** 0.100+  
> **Versión de Python:** 3.10+

---

## Tabla de Contenidos

1. [Introducción a FastAPI](#1-introducción-a-fastapi)
2. [Instalación y Entorno](#2-instalación-y-entorno)
3. [Fundamentos](#3-fundamentos)
4. [Pydantic](#4-pydantic)
5. [Documentación Automática](#5-documentación-automática)
6. [Arquitectura Profesional](#6-arquitectura-profesional)
7. [Base de Datos](#7-base-de-datos)
8. [Autenticación y Seguridad](#8-autenticación-y-seguridad)
9. [Programación Asíncrona](#9-programación-asíncrona)
10. [Testing](#10-testing)
11. [Deployment](#11-deployment)
12. [Buenas Prácticas](#12-buenas-prácticas)
13. [Middleware, CORS y Eventos del Ciclo de Vida](#13-middleware-cors-y-eventos-del-ciclo-de-vida)
14. [Proyecto Final](#14-proyecto-final)

---

# 1. Introducción a FastAPI

## 1.1 ¿Qué es FastAPI?

FastAPI es un framework web moderno y de alto rendimiento para construir APIs con Python, basado en los estándares **OpenAPI** y **JSON Schema**. Fue creado por **Sebastián Ramírez** y publicado por primera vez en diciembre de 2018.

Se fundamenta en dos pilares tecnológicos:

- **Starlette:** Para el manejo de peticiones HTTP y WebSockets.
- **Pydantic:** Para la validación y serialización de datos mediante type hints de Python.

### Características principales

| Característica | Descripción |
|---|---|
| **Alto rendimiento** | Comparable a Node.js y Go, gracias a Starlette y Uvicorn |
| **Tipado estricto** | Uso intensivo de type hints de Python 3.10+ |
| **Documentación automática** | Genera Swagger UI y ReDoc automáticamente |
| **Validación automática** | Validación de datos de entrada y salida vía Pydantic |
| **Soporte asíncrono nativo** | Compatible con `async`/`await` de forma nativa |
| **Estándares abiertos** | Basado en OpenAPI 3.1 y JSON Schema |

## 1.2 Historia y Propósito

FastAPI nació de la necesidad de tener un framework que combinara:

1. La **velocidad de desarrollo** de Flask.
2. La **robustez** de Django REST Framework.
3. El **rendimiento** de frameworks como Starlette.
4. La **seguridad de tipos** que ofrecen los type hints modernos de Python.

Sebastián Ramírez diseñó FastAPI después de trabajar con múltiples frameworks y encontrar que ninguno ofrecía todas estas cualidades de forma integrada.

## 1.3 Comparativa con Flask y Django

| Aspecto | FastAPI | Flask | Django |
|---|---|---|---|
| **Rendimiento** | Muy alto (asíncrono) | Medio (síncrono) | Medio (síncrono) |
| **Validación de datos** | Automática (Pydantic) | Manual | DRF Serializers |
| **Documentación API** | Automática (Swagger/ReDoc) | Manual (Flask-Swagger) | Manual (drf-spectacular) |
| **Curva de aprendizaje** | Baja-Media | Baja | Alta |
| **ORM incluido** | No (usa SQLAlchemy) | No | Sí (Django ORM) |
| **Soporte async** | Nativo | Limitado | Parcial (Django 4.1+) |
| **Ecosistema** | En crecimiento | Maduro | Muy maduro |
| **Admin panel** | No incluido | No incluido | Incluido |
| **Ideal para** | APIs REST, microservicios | Aplicaciones ligeras, prototipos | Aplicaciones monolíticas completas |

## 1.4 Casos de Uso

- **APIs RESTful** de alto rendimiento.
- **Microservicios** en arquitecturas distribuidas.
- **Backends** para aplicaciones móviles y SPA.
- **Sistemas de Machine Learning** que exponen modelos como API.
- **Plataformas de datos en tiempo real** con WebSockets.
- **Gateways y proxies** de APIs.

## 1.5 Ventajas y Desventajas

### Ventajas

- Rendimiento excepcional entre los frameworks de Python.
- Reducción de errores humanos gracias al tipado estricto.
- Documentación interactiva generada automáticamente.
- Validación y serialización integrada sin configuración adicional.
- Soporte nativo para operaciones asíncronas.
- Comunidad activa y en rápido crecimiento.

### Desventajas

- Ecosistema más pequeño comparado con Django o Flask.
- No incluye ORM ni panel de administración.
- Requiere conocimientos sólidos de type hints de Python.
- Menor cantidad de tutoriales y recursos en comparación con frameworks más maduros.
- Para aplicaciones monolíticas complejas, Django puede ser más adecuado.

### Ejercicio Práctico

> Investigar y documentar tres APIs públicas conocidas que utilicen FastAPI. Consultar el repositorio oficial de FastAPI en GitHub para encontrar empresas y proyectos que lo utilizan en producción.

### Reto Opcional

> Crear un documento comparativo propio que evalúe FastAPI, Flask y Django para un proyecto específico de la elección del lector, considerando requisitos técnicos reales.

### Resumen del Capítulo

FastAPI es un framework moderno, rápido y basado en estándares que combina lo mejor de los frameworks existentes en Python. Su uso de type hints, validación automática y documentación generada lo convierten en una opción ideal para el desarrollo de APIs profesionales. Aunque su ecosistema es más joven, su rendimiento y ergonomía lo posicionan como una de las mejores opciones actuales para APIs en Python.

---

# 2. Instalación y Entorno

## 2.1 Requisitos Previos

- **Python 3.10** o superior instalado.
- **pip** como gestor de paquetes.
- Un editor de código (se recomienda VS Code con la extensión Pylance).
- Conocimientos básicos de terminal/línea de comandos.

## 2.2 Creación del Entorno Virtual

Es una práctica obligatoria trabajar con entornos virtuales para aislar las dependencias de cada proyecto.

```bash
# Crear el directorio del proyecto
mkdir mi-api && cd mi-api

# Crear el entorno virtual
python3 -m venv venv

# Activar el entorno virtual
# En macOS/Linux:
source venv/bin/activate

# En Windows:
# venv\Scripts\activate

# Verificar que el entorno está activo
which python
# Debería mostrar: /ruta/al/proyecto/venv/bin/python
```

## 2.3 Instalación de FastAPI

```bash
# Instalación completa (incluye uvicorn y dependencias opcionales)
pip install "fastapi[standard]"

# O instalación mínima + uvicorn por separado
pip install fastapi uvicorn

# Generar archivo de dependencias
pip freeze > requirements.txt
```

### Paquetes instalados principales

| Paquete | Función |
|---|---|
| `fastapi` | Framework principal |
| `uvicorn` | Servidor ASGI de alto rendimiento |
| `pydantic` | Validación y serialización de datos |
| `starlette` | Toolkit ASGI subyacente |
| `typing-extensions` | Extensiones de tipado para compatibilidad |

## 2.4 Estructura Inicial del Proyecto

```
mi-api/
├── venv/                  # Entorno virtual (no se sube al repositorio)
├── app/
│   ├── __init__.py        # Hace del directorio un paquete Python
│   └── main.py            # Punto de entrada de la aplicación
├── requirements.txt       # Dependencias del proyecto
├── .gitignore             # Archivos a excluir del control de versiones
└── README.md              # Documentación del proyecto
```

### Contenido de `.gitignore`

```gitignore
venv/
__pycache__/
*.pyc
.env
.idea/
.vscode/
```

## 2.5 Primera Aplicación

Crear el archivo `app/main.py`:

```python
"""Punto de entrada principal de la aplicación FastAPI."""

from fastapi import FastAPI

# Crear la instancia de la aplicación
app = FastAPI(
    title="Mi Primera API",
    description="API de ejemplo para el curso de FastAPI",
    version="1.0.0",
)


@app.get("/")
async def root():
    """Endpoint raíz que retorna un mensaje de bienvenida."""
    return {"mensaje": "¡Bienvenido a FastAPI!"}


@app.get("/salud")
async def health_check():
    """Endpoint de verificación de salud del servicio."""
    return {"estado": "activo"}
```

## 2.6 Ejecución del Servidor

```bash
# Ejecución básica
uvicorn app.main:app

# Ejecución con recarga automática (desarrollo)
uvicorn app.main:app --reload

# Ejecución especificando host y puerto
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Desglose del comando

| Parte | Significado |
|---|---|
| `uvicorn` | Servidor ASGI |
| `app.main` | Módulo Python (`app/main.py`) |
| `:app` | Variable que contiene la instancia de `FastAPI()` |
| `--reload` | Recarga automática al detectar cambios en el código |
| `--host 0.0.0.0` | Acepta conexiones desde cualquier interfaz de red |
| `--port 8000` | Puerto en el que escucha el servidor |

### Verificación

Una vez ejecutado, se puede acceder a:

- **API:** `http://127.0.0.1:8000`
- **Swagger UI:** `http://127.0.0.1:8000/docs`
- **ReDoc:** `http://127.0.0.1:8000/redoc`

## 2.7 Hot Reload

El flag `--reload` activa la recarga automática del servidor cada vez que se detecta un cambio en los archivos del proyecto. Esto es extremadamente útil durante el desarrollo, ya que elimina la necesidad de reiniciar manualmente el servidor.

**Consideraciones:**

- Utilizar `--reload` **únicamente** en entornos de desarrollo.
- En producción, se debe ejecutar el servidor sin este flag para evitar overhead innecesario.
- Es posible especificar directorios a monitorear con `--reload-dir`:

```bash
uvicorn app.main:app --reload --reload-dir app
```

### Ejercicio Práctico

> Crear un proyecto nuevo desde cero siguiendo la estructura propuesta. Implementar los dos endpoints del ejemplo, ejecutar el servidor con `--reload` y verificar que tanto la API como la documentación Swagger están accesibles.

### Reto Opcional

> Añadir un archivo `Makefile` con comandos para: instalar dependencias, ejecutar el servidor en modo desarrollo y ejecutar el servidor en modo producción.

### Resumen del Capítulo

La instalación de FastAPI es sencilla y rápida. El uso de entornos virtuales es obligatorio para mantener las dependencias aisladas. La estructura de proyecto propuesta permite escalar de forma ordenada. Uvicorn actúa como el servidor ASGI que ejecuta la aplicación, y el flag `--reload` facilita el desarrollo iterativo.

---

# 3. Fundamentos

## 3.1 Rutas (Endpoints)

En FastAPI, una ruta es la combinación de una **URL** y un **método HTTP** asociado a una función Python. Estas funciones se denominan **path operation functions**.

```python
from fastapi import FastAPI

app = FastAPI()


# Ruta GET en la raíz
@app.get("/")
async def root():
    return {"mensaje": "Hola Mundo"}


# Ruta GET con path específico
@app.get("/usuarios")
async def listar_usuarios():
    return {"usuarios": ["Ana", "Luis", "Carlos"]}


# Ruta POST
@app.post("/usuarios")
async def crear_usuario():
    return {"mensaje": "Usuario creado"}
```

## 3.2 Métodos HTTP

FastAPI soporta todos los métodos HTTP estándar:

| Método | Decorador | Uso típico |
|---|---|---|
| GET | `@app.get()` | Obtener recursos |
| POST | `@app.post()` | Crear recursos |
| PUT | `@app.put()` | Actualizar un recurso completo |
| PATCH | `@app.patch()` | Actualizar parcialmente un recurso |
| DELETE | `@app.delete()` | Eliminar recursos |
| OPTIONS | `@app.options()` | Consultar métodos permitidos |
| HEAD | `@app.head()` | Obtener solo headers |

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def obtener_item(item_id: int):
    return {"item_id": item_id}


@app.post("/items", status_code=201)
async def crear_item():
    return {"mensaje": "Item creado exitosamente"}


@app.put("/items/{item_id}")
async def actualizar_item(item_id: int):
    return {"mensaje": f"Item {item_id} actualizado"}


@app.patch("/items/{item_id}")
async def actualizar_parcial_item(item_id: int):
    return {"mensaje": f"Item {item_id} parcialmente actualizado"}


@app.delete("/items/{item_id}", status_code=204)
async def eliminar_item(item_id: int):
    return None
```

## 3.3 Parámetros de Ruta (Path Parameters)

Los parámetros de ruta se definen dentro de la URL usando llaves `{}` y se reciben como argumentos de la función.

```python
from fastapi import FastAPI, Path

app = FastAPI()


# Parámetro de ruta básico
@app.get("/usuarios/{usuario_id}")
async def obtener_usuario(usuario_id: int):
    """El tipo `int` garantiza que FastAPI valide que el valor sea un entero."""
    return {"usuario_id": usuario_id}


# Parámetro de ruta con validación adicional
@app.get("/productos/{producto_id}")
async def obtener_producto(
    producto_id: int = Path(
        ...,                         # ... indica que es obligatorio
        title="ID del producto",
        description="Identificador único del producto",
        ge=1,                        # Mayor o igual a 1
        le=10000,                    # Menor o igual a 10000
    ),
):
    return {"producto_id": producto_id}


# Múltiples parámetros de ruta
@app.get("/tiendas/{tienda_id}/productos/{producto_id}")
async def obtener_producto_tienda(tienda_id: int, producto_id: int):
    return {
        "tienda_id": tienda_id,
        "producto_id": producto_id,
    }
```

### Orden de las rutas

El orden en que se definen las rutas importa. FastAPI evalúa las rutas de arriba hacia abajo:

```python
# CORRECTO: la ruta fija va antes que la ruta con parámetro
@app.get("/usuarios/me")
async def obtener_usuario_actual():
    return {"usuario": "Usuario actual"}


@app.get("/usuarios/{usuario_id}")
async def obtener_usuario(usuario_id: int):
    return {"usuario_id": usuario_id}
```

### Enum como parámetro de ruta

```python
from enum import Enum

from fastapi import FastAPI


class EstadoPedido(str, Enum):
    """Enumeración de estados posibles de un pedido."""
    PENDIENTE = "pendiente"
    ENVIADO = "enviado"
    ENTREGADO = "entregado"
    CANCELADO = "cancelado"


app = FastAPI()


@app.get("/pedidos/estado/{estado}")
async def obtener_pedidos_por_estado(estado: EstadoPedido):
    return {
        "estado": estado,
        "valor": estado.value,
    }
```

## 3.4 Parámetros de Consulta (Query Parameters)

Los parámetros de consulta son aquellos que no forman parte de la ruta y se pasan en la URL después del signo `?`.

```python
from fastapi import FastAPI, Query

app = FastAPI()


# Query params básicos
@app.get("/items")
async def listar_items(
    pagina: int = 1,
    limite: int = 10,
):
    """
    Ejemplo de uso: GET /items?pagina=2&limite=20
    Los valores por defecto hacen que los parámetros sean opcionales.
    """
    return {
        "pagina": pagina,
        "limite": limite,
        "offset": (pagina - 1) * limite,
    }


# Query params con validación avanzada
@app.get("/buscar")
async def buscar(
    q: str = Query(
        ...,                            # Obligatorio (sin valor por defecto)
        min_length=3,                   # Mínimo 3 caracteres
        max_length=100,                 # Máximo 100 caracteres
        pattern="^[a-zA-Z0-9 ]+$",     # Solo alfanuméricos y espacios
        title="Término de búsqueda",
        description="Texto a buscar en los registros",
    ),
    categoria: str | None = Query(
        default=None,                   # Opcional
        description="Filtrar por categoría",
    ),
    ordenar_por: str = Query(
        default="fecha",
        description="Campo por el cual ordenar los resultados",
    ),
):
    resultados = {"busqueda": q, "categoria": categoria, "orden": ordenar_por}
    return resultados


# Query params como lista
@app.get("/filtrar")
async def filtrar(
    etiquetas: list[str] = Query(
        default=[],
        description="Lista de etiquetas para filtrar",
    ),
):
    """
    Ejemplo de uso: GET /filtrar?etiquetas=python&etiquetas=fastapi
    """
    return {"etiquetas": etiquetas}
```

## 3.5 Cuerpo de la Petición (Request Body)

Para recibir datos en el cuerpo de la petición, se utilizan modelos de Pydantic.

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr, Field


class UsuarioCrear(BaseModel):
    """Modelo para la creación de un usuario."""
    nombre: str = Field(
        ...,
        min_length=2,
        max_length=50,
        examples=["Juan Pérez"],
    )
    email: EmailStr = Field(
        ...,
        examples=["juan@ejemplo.com"],
    )
    edad: int = Field(
        ...,
        ge=18,
        le=120,
        description="Edad del usuario (debe ser mayor de edad)",
    )
    activo: bool = Field(default=True)


class UsuarioRespuesta(BaseModel):
    """Modelo de respuesta al crear un usuario."""
    id: int
    nombre: str
    email: EmailStr
    activo: bool


app = FastAPI()

# Base de datos simulada
usuarios_db: list[dict] = []
contador_id: int = 0


@app.post("/usuarios", response_model=UsuarioRespuesta, status_code=201)
async def crear_usuario(usuario: UsuarioCrear):
    """
    Crea un nuevo usuario.

    FastAPI automáticamente:
    1. Lee el cuerpo de la petición como JSON.
    2. Valida los datos contra el modelo `UsuarioCrear`.
    3. Convierte los tipos de datos según lo definido.
    4. Si hay errores de validación, retorna un 422 con detalles.
    """
    global contador_id
    contador_id += 1

    nuevo_usuario = {
        "id": contador_id,
        **usuario.model_dump(),
    }
    usuarios_db.append(nuevo_usuario)
    return nuevo_usuario
```

### Combinación de parámetros

Es posible combinar path params, query params y body params en una misma función:

```python
from fastapi import FastAPI, Path, Query
from pydantic import BaseModel, Field


class ItemActualizar(BaseModel):
    """Modelo para la actualización de un item."""
    nombre: str | None = Field(default=None, max_length=100)
    precio: float | None = Field(default=None, gt=0)
    descripcion: str | None = Field(default=None, max_length=500)


app = FastAPI()


@app.put("/tiendas/{tienda_id}/items/{item_id}")
async def actualizar_item(
    tienda_id: int = Path(..., ge=1),         # Path parameter
    item_id: int = Path(..., ge=1),           # Path parameter
    item: ItemActualizar = ...,               # Body parameter (modelo Pydantic)
    notificar: bool = Query(default=False),   # Query parameter
):
    """
    Reglas de detección automática de FastAPI:
    - Si el parámetro está en la ruta → path parameter
    - Si el parámetro es un modelo Pydantic → body parameter
    - Si el parámetro es de tipo simple (str, int, etc.) → query parameter
    """
    return {
        "tienda_id": tienda_id,
        "item_id": item_id,
        "datos": item.model_dump(exclude_unset=True),
        "notificar": notificar,
    }
```

## 3.6 Respuestas JSON y Códigos de Estado

```python
from fastapi import FastAPI, HTTPException
from fastapi.responses import JSONResponse
from pydantic import BaseModel


class Mensaje(BaseModel):
    detalle: str


app = FastAPI()


# Respuesta con código de estado personalizado
@app.post("/recursos", status_code=201)
async def crear_recurso():
    return {"mensaje": "Recurso creado"}


# Uso de JSONResponse para control total
@app.get("/personalizado")
async def respuesta_personalizada():
    contenido = {"mensaje": "Respuesta con headers personalizados"}
    return JSONResponse(
        content=contenido,
        status_code=200,
        headers={"X-Custom-Header": "valor-personalizado"},
    )


# Manejo de errores con HTTPException
@app.get("/usuarios/{usuario_id}")
async def obtener_usuario(usuario_id: int):
    usuarios = {1: "Ana", 2: "Luis"}

    if usuario_id not in usuarios:
        raise HTTPException(
            status_code=404,
            detail=f"Usuario con ID {usuario_id} no encontrado",
            headers={"X-Error": "No encontrado"},
        )

    return {"id": usuario_id, "nombre": usuarios[usuario_id]}


# Respuestas documentadas para Swagger
@app.get(
    "/items/{item_id}",
    responses={
        200: {"description": "Item encontrado exitosamente"},
        404: {"model": Mensaje, "description": "Item no encontrado"},
    },
)
async def obtener_item(item_id: int):
    items = {1: "Laptop", 2: "Mouse"}

    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item no encontrado")

    return {"id": item_id, "nombre": items[item_id]}
```

### Diagrama del flujo de una petición

```mermaid
graph TD
    A[Cliente HTTP] -->|Petición| B[Uvicorn - Servidor ASGI]
    B --> C[FastAPI - Router]
    C --> D{¿Ruta encontrada?}
    D -->|No| E[Respuesta 404]
    D -->|Sí| F[Validación de Parámetros]
    F --> G{¿Datos válidos?}
    G -->|No| H[Respuesta 422 - Error de validación]
    G -->|Sí| I[Ejecutar función del endpoint]
    I --> J{¿Excepción?}
    J -->|Sí| K[Respuesta con código de error]
    J -->|No| L[Serialización de respuesta]
    L --> M[Respuesta JSON al cliente]
```

### Ejercicio Práctico

> Construir una API de gestión de tareas (To-Do) con los siguientes endpoints:
>
> - `GET /tareas` — Listar todas las tareas con paginación (query params).
> - `GET /tareas/{tarea_id}` — Obtener una tarea por ID.
> - `POST /tareas` — Crear una tarea (body con modelo Pydantic).
> - `PUT /tareas/{tarea_id}` — Actualizar una tarea.
> - `DELETE /tareas/{tarea_id}` — Eliminar una tarea.
>
> Cada tarea debe tener: `id`, `titulo`, `descripcion`, `completada`, `fecha_creacion`.

### Reto Opcional

> Añadir un endpoint `GET /tareas/buscar` que acepte múltiples query params: `q` (texto), `completada` (bool), `orden` (enum: fecha_asc, fecha_desc) y `limite` (int con validación).

### Resumen del Capítulo

Los fundamentos de FastAPI incluyen el manejo de rutas, métodos HTTP, y tres tipos de parámetros: path, query y body. FastAPI detecta automáticamente el tipo de parámetro según su posición y tipo. La validación se realiza de forma automática mediante type hints y las clases `Path`, `Query` y modelos Pydantic. Las respuestas pueden personalizarse con códigos de estado, headers y documentación para Swagger.

---

# 4. Pydantic

## 4.1 Modelos Básicos

Pydantic es el motor de validación y serialización de FastAPI. Cada modelo hereda de `BaseModel` y utiliza type hints para definir la estructura de los datos.

```python
from datetime import datetime
from pydantic import BaseModel, Field


class Producto(BaseModel):
    """Modelo que representa un producto en el sistema."""
    nombre: str = Field(..., min_length=1, max_length=200)
    descripcion: str | None = Field(default=None, max_length=1000)
    precio: float = Field(..., gt=0, description="Precio en USD")
    cantidad: int = Field(default=0, ge=0)
    categorias: list[str] = Field(default_factory=list)
    disponible: bool = Field(default=True)
    creado_en: datetime = Field(default_factory=datetime.now)


# Crear una instancia desde un diccionario
datos = {
    "nombre": "Laptop Gaming",
    "precio": 1299.99,
    "cantidad": 15,
    "categorias": ["electrónica", "computación"],
}

producto = Producto(**datos)
# O equivalentemente:
producto = Producto.model_validate(datos)

# Serializar a diccionario
print(producto.model_dump())

# Serializar a JSON
print(producto.model_dump_json(indent=2))
```

## 4.2 Validaciones

### Validaciones con Field

```python
from pydantic import BaseModel, Field


class Empleado(BaseModel):
    nombre: str = Field(..., min_length=2, max_length=100)
    edad: int = Field(..., ge=18, le=99)
    salario: float = Field(..., gt=0, le=1_000_000)
    departamento: str = Field(..., pattern="^[A-Z]{2,10}$")
```

### Validadores personalizados

```python
from pydantic import BaseModel, Field, field_validator, model_validator


class RegistroUsuario(BaseModel):
    username: str = Field(..., min_length=3, max_length=30)
    email: str
    password: str = Field(..., min_length=8)
    password_confirmacion: str

    @field_validator("username")
    @classmethod
    def username_sin_espacios(cls, v: str) -> str:
        if " " in v:
            raise ValueError("El username no puede contener espacios")
        return v.lower()

    @field_validator("email")
    @classmethod
    def validar_email(cls, v: str) -> str:
        if "@" not in v:
            raise ValueError("Email inválido")
        return v.lower()

    @model_validator(mode="after")
    def verificar_passwords(self) -> "RegistroUsuario":
        if self.password != self.password_confirmacion:
            raise ValueError("Las contraseñas no coinciden")
        return self
```

## 4.3 Tipado Avanzado

```python
from typing import Annotated, Literal
from pydantic import BaseModel, Field

# Tipos personalizados con Annotated
NombreStr = Annotated[str, Field(min_length=1, max_length=100)]
PrecioFloat = Annotated[float, Field(gt=0, le=999999.99)]

class Pedido(BaseModel):
    cliente: NombreStr
    total: PrecioFloat
    estado: Literal["pendiente", "procesando", "enviado", "entregado"]
    prioridad: int = Field(default=0, ge=0, le=5)
```

## 4.4 Serialización y Configuración

```python
from pydantic import BaseModel, Field, ConfigDict


class UsuarioDB(BaseModel):
    """Modelo con configuración avanzada."""
    model_config = ConfigDict(
        from_attributes=True,       # Permite crear desde objetos ORM
        str_strip_whitespace=True,  # Elimina espacios al inicio/final
        str_min_length=1,           # Longitud mínima global para strings
    )

    id: int
    nombre: str
    email: str
    password_hash: str = Field(exclude=True)  # Excluir de serialización

    @property
    def nombre_display(self) -> str:
        return self.nombre.title()
```

### Modelos anidados

```python
from pydantic import BaseModel, Field


class Direccion(BaseModel):
    calle: str
    ciudad: str
    codigo_postal: str
    pais: str = "México"


class Empresa(BaseModel):
    nombre: str
    direccion: Direccion
    empleados: list[str] = Field(default_factory=list)


# Uso
datos = {
    "nombre": "TechCorp",
    "direccion": {
        "calle": "Av. Reforma 123",
        "ciudad": "CDMX",
        "codigo_postal": "06600",
    },
    "empleados": ["Ana", "Luis"],
}

empresa = Empresa.model_validate(datos)
```

## 4.5 Validaciones Avanzadas en FastAPI

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field, field_validator

app = FastAPI()


class ProductoCrear(BaseModel):
    nombre: str = Field(..., min_length=1, max_length=200)
    precio: float = Field(..., gt=0)
    descuento: float = Field(default=0, ge=0, le=100)

    @field_validator("nombre")
    @classmethod
    def capitalizar_nombre(cls, v: str) -> str:
        return v.strip().title()


class ProductoRespuesta(BaseModel):
    id: int
    nombre: str
    precio: float
    precio_final: float


@app.post("/productos", response_model=ProductoRespuesta, status_code=201)
async def crear_producto(producto: ProductoCrear):
    precio_final = producto.precio * (1 - producto.descuento / 100)
    return {
        "id": 1,
        "nombre": producto.nombre,
        "precio": producto.precio,
        "precio_final": round(precio_final, 2),
    }
```

### Ejercicio Práctico

> Crear modelos Pydantic para un sistema de e-commerce que incluya: `Producto`, `Carrito`, `ItemCarrito` y `Orden`. Implementar validadores personalizados para verificar que el stock sea suficiente y que el total del carrito sea correcto.

### Reto Opcional

> Implementar un validador genérico reutilizable que sanitice strings (elimine HTML, recorte espacios, valide longitud) y aplicarlo a múltiples campos usando `Annotated`.

### Resumen del Capítulo

Pydantic proporciona un sistema robusto de validación y serialización basado en type hints. Los modelos permiten definir estructuras de datos con validaciones automáticas y personalizadas. La configuración avanzada permite adaptar el comportamiento de serialización, y los modelos anidados permiten representar estructuras de datos complejas.

---

# 5. Documentación Automática

## 5.1 OpenAPI

FastAPI genera automáticamente un esquema OpenAPI 3.1 completo basado en las rutas, modelos y type hints definidos. Este esquema está disponible en `/openapi.json`.

```python
from fastapi import FastAPI

app = FastAPI(
    title="API de Gestión de Inventario",
    description="""
    ## API para gestión de inventario empresarial
    
    Esta API permite:
    * Gestionar productos
    * Controlar stock
    * Generar reportes
    """,
    version="2.0.0",
    contact={
        "name": "Equipo de Desarrollo",
        "email": "dev@empresa.com",
    },
    license_info={
        "name": "MIT",
    },
)
```

## 5.2 Swagger UI

Disponible en `/docs` por defecto. Permite:

- Visualizar todos los endpoints de la API.
- Probar endpoints directamente desde el navegador.
- Ver los modelos de datos (schemas).
- Autenticarse con tokens para probar endpoints protegidos.

### Personalización de la documentación por endpoint

```python
from fastapi import FastAPI, Path, Query
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    nombre: str
    precio: float

    model_config = {
        "json_schema_extra": {
            "examples": [
                {"nombre": "Laptop", "precio": 999.99},
            ]
        }
    }


@app.post(
    "/items",
    summary="Crear un nuevo item",
    description="Crea un item en el inventario con validación automática.",
    response_description="El item creado exitosamente",
    tags=["Items"],
    status_code=201,
    responses={
        201: {"description": "Item creado"},
        422: {"description": "Error de validación"},
    },
)
async def crear_item(item: Item):
    """
    Crea un item con los siguientes datos:

    - **nombre**: nombre del item (obligatorio)
    - **precio**: precio en USD (debe ser positivo)
    """
    return {"id": 1, **item.model_dump()}
```

## 5.3 ReDoc

Disponible en `/redoc`. Ofrece una vista alternativa de la documentación, más orientada a la lectura y con mejor soporte para documentación extensa.

## 5.4 Organización con Tags

```python
from fastapi import FastAPI
from enum import Enum


class Tags(str, Enum):
    USUARIOS = "Usuarios"
    PRODUCTOS = "Productos"
    ORDENES = "Órdenes"


app = FastAPI()


@app.get("/usuarios", tags=[Tags.USUARIOS])
async def listar_usuarios():
    return []


@app.get("/productos", tags=[Tags.PRODUCTOS])
async def listar_productos():
    return []
```

## 5.5 Deshabilitar Documentación

En producción, puede ser deseable deshabilitar la documentación:

```python
from fastapi import FastAPI

app = FastAPI(
    docs_url=None,      # Deshabilita Swagger
    redoc_url=None,     # Deshabilita ReDoc
    openapi_url=None,   # Deshabilita el schema OpenAPI
)
```

### Ejercicio Práctico

> Crear una API con al menos 3 grupos de tags, documentar cada endpoint con `summary`, `description` y `responses`. Verificar la documentación generada en Swagger y ReDoc.

### Reto Opcional

> Configurar la documentación para que solo esté disponible en entornos de desarrollo usando variables de entorno.

### Resumen del Capítulo

FastAPI genera documentación interactiva automáticamente basada en el código. Swagger UI y ReDoc proporcionan interfaces para explorar y probar la API. La documentación se personaliza mediante parámetros en los decoradores, docstrings, tags y configuración de la instancia de FastAPI. En producción, es recomendable evaluar si la documentación debe estar expuesta.

---

# 6. Arquitectura Profesional

## 6.1 Separación por Capas

Una arquitectura profesional separa responsabilidades en capas bien definidas:

```mermaid
graph TD
    A[Cliente] --> B[Router / Controller]
    B --> C[Service - Lógica de negocio]
    C --> D[Repository - Acceso a datos]
    D --> E[Base de Datos]
    B --> F[Schema - Validación]
    C --> F
```

## 6.2 Estructura de Proyecto Profesional

```
proyecto/
├── app/
│   ├── __init__.py
│   ├── main.py                    # Punto de entrada
│   ├── config.py                  # Configuración centralizada
│   ├── database.py                # Conexión a base de datos
│   ├── dependencies.py            # Dependencias compartidas
│   ├── exceptions.py              # Excepciones personalizadas
│   ├── models/                    # Modelos ORM (SQLAlchemy)
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   └── producto.py
│   ├── schemas/                   # Modelos Pydantic
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   └── producto.py
│   ├── routers/                   # Endpoints agrupados
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   └── producto.py
│   ├── services/                  # Lógica de negocio
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   └── producto.py
│   └── repositories/             # Acceso a datos
│       ├── __init__.py
│       ├── usuario.py
│       └── producto.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   └── test_usuarios.py
├── alembic/                       # Migraciones
├── alembic.ini
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
└── .env
```

## 6.3 Configuración Centralizada

```python
# app/config.py
from functools import lru_cache
from pydantic_settings import BaseSettings


class Settings(BaseSettings):
    """Configuración centralizada cargada desde variables de entorno."""
    app_name: str = "Mi API"
    debug: bool = False
    database_url: str
    secret_key: str
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30

    model_config = {"env_file": ".env"}


@lru_cache
def get_settings() -> Settings:
    """Retorna la configuración cacheada (singleton)."""
    return Settings()
```

## 6.4 Routers

Los routers permiten organizar los endpoints en módulos independientes.

```python
# app/routers/usuario.py
from fastapi import APIRouter, Depends, HTTPException, status
from app.schemas.usuario import UsuarioCrear, UsuarioRespuesta
from app.services.usuario import UsuarioService
from app.dependencies import get_usuario_service

router = APIRouter(
    prefix="/usuarios",
    tags=["Usuarios"],
    responses={404: {"description": "No encontrado"}},
)


@router.get("/", response_model=list[UsuarioRespuesta])
async def listar_usuarios(
    skip: int = 0,
    limit: int = 100,
    servicio: UsuarioService = Depends(get_usuario_service),
):
    return await servicio.obtener_todos(skip=skip, limit=limit)


@router.post("/", response_model=UsuarioRespuesta, status_code=201)
async def crear_usuario(
    usuario: UsuarioCrear,
    servicio: UsuarioService = Depends(get_usuario_service),
):
    return await servicio.crear(usuario)


@router.get("/{usuario_id}", response_model=UsuarioRespuesta)
async def obtener_usuario(
    usuario_id: int,
    servicio: UsuarioService = Depends(get_usuario_service),
):
    usuario = await servicio.obtener_por_id(usuario_id)
    if not usuario:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Usuario no encontrado",
        )
    return usuario
```

### Registro de routers en main.py

```python
# app/main.py
from fastapi import FastAPI
from app.routers import usuario, producto
from app.config import get_settings

settings = get_settings()

app = FastAPI(title=settings.app_name, debug=settings.debug)

# Registrar routers
app.include_router(usuario.router)
app.include_router(producto.router)


@app.get("/")
async def root():
    return {"app": settings.app_name, "estado": "activo"}
```

## 6.5 Schemas (Modelos Pydantic)

```python
# app/schemas/usuario.py
from datetime import datetime
from pydantic import BaseModel, EmailStr, Field, ConfigDict


class UsuarioBase(BaseModel):
    """Campos compartidos entre creación y respuesta."""
    nombre: str = Field(..., min_length=2, max_length=100)
    email: EmailStr


class UsuarioCrear(UsuarioBase):
    """Schema para crear un usuario."""
    password: str = Field(..., min_length=8, max_length=128)


class UsuarioActualizar(BaseModel):
    """Schema para actualizar parcialmente un usuario."""
    nombre: str | None = Field(default=None, min_length=2, max_length=100)
    email: EmailStr | None = None


class UsuarioRespuesta(UsuarioBase):
    """Schema de respuesta (nunca expone el password)."""
    model_config = ConfigDict(from_attributes=True)

    id: int
    activo: bool
    creado_en: datetime
```

## 6.6 Services (Lógica de Negocio)

```python
# app/services/usuario.py
from app.repositories.usuario import UsuarioRepository
from app.schemas.usuario import UsuarioCrear, UsuarioActualizar


class UsuarioService:
    """Capa de lógica de negocio para usuarios."""

    def __init__(self, repositorio: UsuarioRepository):
        self.repositorio = repositorio

    async def obtener_todos(self, skip: int = 0, limit: int = 100):
        return await self.repositorio.obtener_todos(skip, limit)

    async def obtener_por_id(self, usuario_id: int):
        return await self.repositorio.obtener_por_id(usuario_id)

    async def crear(self, datos: UsuarioCrear):
        # Verificar que el email no exista
        existente = await self.repositorio.obtener_por_email(datos.email)
        if existente:
            raise ValueError("El email ya está registrado")

        # Aquí se hashearía el password
        return await self.repositorio.crear(datos)

    async def actualizar(self, usuario_id: int, datos: UsuarioActualizar):
        return await self.repositorio.actualizar(usuario_id, datos)

    async def eliminar(self, usuario_id: int):
        return await self.repositorio.eliminar(usuario_id)
```

## 6.7 Repositories (Acceso a Datos)

```python
# app/repositories/usuario.py
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select
from app.models.usuario import Usuario
from app.schemas.usuario import UsuarioCrear, UsuarioActualizar


class UsuarioRepository:
    """Capa de acceso a datos para usuarios."""

    def __init__(self, db: AsyncSession):
        self.db = db

    async def obtener_todos(self, skip: int = 0, limit: int = 100):
        resultado = await self.db.execute(
            select(Usuario).offset(skip).limit(limit)
        )
        return resultado.scalars().all()

    async def obtener_por_id(self, usuario_id: int):
        resultado = await self.db.execute(
            select(Usuario).where(Usuario.id == usuario_id)
        )
        return resultado.scalar_one_or_none()

    async def obtener_por_email(self, email: str):
        resultado = await self.db.execute(
            select(Usuario).where(Usuario.email == email)
        )
        return resultado.scalar_one_or_none()

    async def crear(self, datos: UsuarioCrear):
        usuario = Usuario(**datos.model_dump())
        self.db.add(usuario)
        await self.db.commit()
        await self.db.refresh(usuario)
        return usuario

    async def actualizar(self, usuario_id: int, datos: UsuarioActualizar):
        usuario = await self.obtener_por_id(usuario_id)
        if not usuario:
            return None
        for campo, valor in datos.model_dump(exclude_unset=True).items():
            setattr(usuario, campo, valor)
        await self.db.commit()
        await self.db.refresh(usuario)
        return usuario

    async def eliminar(self, usuario_id: int) -> bool:
        usuario = await self.obtener_por_id(usuario_id)
        if not usuario:
            return False
        await self.db.delete(usuario)
        await self.db.commit()
        return True
```

## 6.8 Inyección de Dependencias

```python
# app/dependencies.py
from typing import AsyncGenerator
from fastapi import Depends
from sqlalchemy.ext.asyncio import AsyncSession
from app.database import async_session_maker
from app.repositories.usuario import UsuarioRepository
from app.services.usuario import UsuarioService


async def get_db() -> AsyncGenerator[AsyncSession, None]:
    """Provee una sesión de base de datos por petición."""
    async with async_session_maker() as session:
        yield session


def get_usuario_repository(
    db: AsyncSession = Depends(get_db),
) -> UsuarioRepository:
    return UsuarioRepository(db)


def get_usuario_service(
    repositorio: UsuarioRepository = Depends(get_usuario_repository),
) -> UsuarioService:
    return UsuarioService(repositorio)
```

```mermaid
graph LR
    A[Router] -->|Depends| B[Service]
    B -->|Depends| C[Repository]
    C -->|Depends| D[DB Session]
```

## 6.9 Excepciones Personalizadas

```python
# app/exceptions.py
from fastapi import HTTPException, status


class NotFoundException(HTTPException):
    def __init__(self, recurso: str, id: int | str):
        super().__init__(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"{recurso} con ID '{id}' no encontrado",
        )


class ConflictException(HTTPException):
    def __init__(self, detalle: str):
        super().__init__(
            status_code=status.HTTP_409_CONFLICT,
            detail=detalle,
        )


class ForbiddenException(HTTPException):
    def __init__(self):
        super().__init__(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="No tiene permisos para realizar esta acción",
        )
```

### Ejercicio Práctico

> Refactorizar la API de tareas (To-Do) del capítulo anterior para que siga la arquitectura profesional con routers, services, repositories, schemas y configuración centralizada.

### Reto Opcional

> Implementar un sistema de plugins donde nuevos módulos (routers) se registren automáticamente al ser añadidos al directorio `routers/`, sin necesidad de modificar `main.py`.

### Resumen del Capítulo

Una arquitectura profesional en FastAPI separa el código en capas: routers (endpoints), services (lógica), repositories (datos) y schemas (validación). La inyección de dependencias de FastAPI (`Depends`) permite conectar estas capas de forma limpia y testeable. La configuración centralizada con `pydantic-settings` facilita el manejo de variables de entorno. Esta estructura es escalable y mantenible para proyectos de cualquier tamaño.

---

# 7. Base de Datos

## 7.1 SQLAlchemy con FastAPI

SQLAlchemy es el ORM más utilizado con FastAPI. Se recomienda su versión asíncrona para aprovechar al máximo el modelo async de FastAPI.

### Instalación

```bash
pip install sqlalchemy asyncpg aiosqlite alembic
# asyncpg: driver asíncrono para PostgreSQL
# aiosqlite: driver asíncrono para SQLite (desarrollo)
```

## 7.2 Configuración de la Base de Datos

```python
# app/database.py
from sqlalchemy.ext.asyncio import (
    AsyncSession,
    async_sessionmaker,
    create_async_engine,
)
from sqlalchemy.orm import DeclarativeBase
from app.config import get_settings

settings = get_settings()

# Motor asíncrono
engine = create_async_engine(
    settings.database_url,
    echo=settings.debug,      # Muestra queries SQL en modo debug
    pool_size=20,              # Conexiones en el pool
    max_overflow=10,           # Conexiones adicionales permitidas
)

# Fábrica de sesiones
async_session_maker = async_sessionmaker(
    engine,
    class_=AsyncSession,
    expire_on_commit=False,
)


class Base(DeclarativeBase):
    """Clase base para todos los modelos ORM."""
    pass


async def init_db():
    """Crea las tablas en la base de datos (solo para desarrollo)."""
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
```

## 7.3 Modelos ORM

```python
# app/models/usuario.py
from datetime import datetime
from sqlalchemy import String, Boolean, DateTime, func
from sqlalchemy.orm import Mapped, mapped_column, relationship
from app.database import Base


class Usuario(Base):
    __tablename__ = "usuarios"

    id: Mapped[int] = mapped_column(primary_key=True, autoincrement=True)
    nombre: Mapped[str] = mapped_column(String(100), nullable=False)
    email: Mapped[str] = mapped_column(String(255), unique=True, nullable=False, index=True)
    password_hash: Mapped[str] = mapped_column(String(255), nullable=False)
    activo: Mapped[bool] = mapped_column(Boolean, default=True)
    creado_en: Mapped[datetime] = mapped_column(DateTime, server_default=func.now())
    actualizado_en: Mapped[datetime] = mapped_column(
        DateTime, server_default=func.now(), onupdate=func.now()
    )

    # Relación uno a muchos
    publicaciones: Mapped[list["Publicacion"]] = relationship(
        back_populates="autor", cascade="all, delete-orphan"
    )

    def __repr__(self) -> str:
        return f"<Usuario(id={self.id}, email='{self.email}')>"
```

```python
# app/models/publicacion.py
from datetime import datetime
from sqlalchemy import String, Text, ForeignKey, DateTime, func
from sqlalchemy.orm import Mapped, mapped_column, relationship
from app.database import Base


class Publicacion(Base):
    __tablename__ = "publicaciones"

    id: Mapped[int] = mapped_column(primary_key=True, autoincrement=True)
    titulo: Mapped[str] = mapped_column(String(200), nullable=False)
    contenido: Mapped[str] = mapped_column(Text, nullable=False)
    autor_id: Mapped[int] = mapped_column(ForeignKey("usuarios.id"), nullable=False)
    creado_en: Mapped[datetime] = mapped_column(DateTime, server_default=func.now())

    # Relación inversa
    autor: Mapped["Usuario"] = relationship(back_populates="publicaciones")
```

### Diagrama de relaciones

```mermaid
erDiagram
    USUARIOS ||--o{ PUBLICACIONES : "tiene"
    USUARIOS {
        int id PK
        string nombre
        string email UK
        string password_hash
        bool activo
        datetime creado_en
    }
    PUBLICACIONES {
        int id PK
        string titulo
        text contenido
        int autor_id FK
        datetime creado_en
    }
```

## 7.4 CRUD Completo Asíncrono

```python
# app/repositories/usuario.py
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select, update, delete
from sqlalchemy.orm import selectinload
from app.models.usuario import Usuario
from app.schemas.usuario import UsuarioCrear, UsuarioActualizar


class UsuarioRepository:
    def __init__(self, db: AsyncSession):
        self.db = db

    async def crear(self, datos: UsuarioCrear, password_hash: str) -> Usuario:
        usuario = Usuario(
            nombre=datos.nombre,
            email=datos.email,
            password_hash=password_hash,
        )
        self.db.add(usuario)
        await self.db.commit()
        await self.db.refresh(usuario)
        return usuario

    async def obtener_por_id(self, usuario_id: int) -> Usuario | None:
        resultado = await self.db.execute(
            select(Usuario)
            .where(Usuario.id == usuario_id)
            .options(selectinload(Usuario.publicaciones))  # Carga eager
        )
        return resultado.scalar_one_or_none()

    async def obtener_todos(
        self, skip: int = 0, limit: int = 100
    ) -> list[Usuario]:
        resultado = await self.db.execute(
            select(Usuario)
            .offset(skip)
            .limit(limit)
            .order_by(Usuario.creado_en.desc())
        )
        return list(resultado.scalars().all())

    async def actualizar(
        self, usuario_id: int, datos: UsuarioActualizar
    ) -> Usuario | None:
        valores = datos.model_dump(exclude_unset=True)
        if not valores:
            return await self.obtener_por_id(usuario_id)

        await self.db.execute(
            update(Usuario)
            .where(Usuario.id == usuario_id)
            .values(**valores)
        )
        await self.db.commit()
        return await self.obtener_por_id(usuario_id)

    async def eliminar(self, usuario_id: int) -> bool:
        resultado = await self.db.execute(
            delete(Usuario).where(Usuario.id == usuario_id)
        )
        await self.db.commit()
        return resultado.rowcount > 0
```

## 7.5 Migraciones con Alembic

### Inicialización

```bash
# Inicializar Alembic en el proyecto
alembic init alembic
```

### Configuración

```python
# alembic/env.py (sección relevante)
from app.database import Base
from app.models import usuario, publicacion  # Importar todos los modelos

target_metadata = Base.metadata
```

```ini
# alembic.ini (línea relevante)
sqlalchemy.url = postgresql+asyncpg://usuario:password@localhost:5432/mi_db
```

### Comandos de Alembic

```bash
# Crear una migración automática
alembic revision --autogenerate -m "crear tablas iniciales"

# Aplicar migraciones pendientes
alembic upgrade head

# Revertir la última migración
alembic downgrade -1

# Ver el historial de migraciones
alembic history

# Ver la migración actual
alembic current
```

### Ejemplo de archivo de migración generado

```python
"""crear tablas iniciales

Revision ID: a1b2c3d4e5f6
Revises:
Create Date: 2024-01-15 10:30:00.000000
"""
from alembic import op
import sqlalchemy as sa

revision = "a1b2c3d4e5f6"
down_revision = None
branch_labels = None
depends_on = None


def upgrade() -> None:
    op.create_table(
        "usuarios",
        sa.Column("id", sa.Integer(), autoincrement=True, nullable=False),
        sa.Column("nombre", sa.String(length=100), nullable=False),
        sa.Column("email", sa.String(length=255), nullable=False),
        sa.Column("password_hash", sa.String(length=255), nullable=False),
        sa.Column("activo", sa.Boolean(), default=True),
        sa.Column("creado_en", sa.DateTime(), server_default=sa.func.now()),
        sa.PrimaryKeyConstraint("id"),
        sa.UniqueConstraint("email"),
    )
    op.create_index("ix_usuarios_email", "usuarios", ["email"])


def downgrade() -> None:
    op.drop_index("ix_usuarios_email", table_name="usuarios")
    op.drop_table("usuarios")
```

### Ejercicio Práctico

> Configurar una base de datos PostgreSQL (o SQLite para desarrollo), crear los modelos de Usuario y Publicación, ejecutar las migraciones con Alembic y verificar que las tablas se crearon correctamente.

### Reto Opcional

> Implementar relaciones muchos a muchos (por ejemplo, usuarios y roles) usando una tabla intermedia, con su migración correspondiente.

### Resumen del Capítulo

SQLAlchemy proporciona un ORM robusto y flexible para FastAPI. La versión asíncrona permite aprovechar el modelo async del framework. Alembic gestiona las migraciones de forma controlada y versionada. La separación en repositorios permite encapsular toda la lógica de acceso a datos y facilita el testing.

---

# 8. Autenticación y Seguridad

## 8.1 Conceptos Fundamentales

```mermaid
sequenceDiagram
    participant C as Cliente
    participant A as API FastAPI
    participant DB as Base de Datos

    C->>A: POST /auth/login (email, password)
    A->>DB: Buscar usuario por email
    DB-->>A: Usuario encontrado
    A->>A: Verificar password hash
    A->>A: Generar JWT
    A-->>C: 200 OK + access_token

    C->>A: GET /usuarios/me (Authorization: Bearer <token>)
    A->>A: Decodificar y validar JWT
    A->>DB: Obtener datos del usuario
    DB-->>A: Datos del usuario
    A-->>C: 200 OK + datos del usuario
```

## 8.2 Hashing de Contraseñas

```bash
pip install passlib[bcrypt]
```

```python
# app/core/security.py
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")


def hash_password(password: str) -> str:
    """Genera un hash bcrypt de la contraseña."""
    return pwd_context.hash(password)


def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Verifica que una contraseña coincida con su hash."""
    return pwd_context.verify(plain_password, hashed_password)
```

## 8.3 JSON Web Tokens (JWT)

```bash
pip install python-jose[cryptography]
```

```python
# app/core/jwt.py
from datetime import datetime, timedelta, timezone
from jose import JWTError, jwt
from app.config import get_settings

settings = get_settings()


def crear_token_acceso(datos: dict, expira_en: timedelta | None = None) -> str:
    """Crea un JWT con los datos proporcionados."""
    a_codificar = datos.copy()
    expiracion = datetime.now(timezone.utc) + (
        expira_en or timedelta(minutes=settings.access_token_expire_minutes)
    )
    a_codificar.update({"exp": expiracion})
    return jwt.encode(a_codificar, settings.secret_key, algorithm=settings.algorithm)


def verificar_token(token: str) -> dict | None:
    """Decodifica y verifica un JWT. Retorna None si es inválido."""
    try:
        payload = jwt.decode(token, settings.secret_key, algorithms=[settings.algorithm])
        return payload
    except JWTError:
        return None
```

## 8.4 OAuth2 con FastAPI

```python
# app/core/auth.py
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
from sqlalchemy.ext.asyncio import AsyncSession
from app.core.jwt import verificar_token
from app.dependencies import get_db
from app.models.usuario import Usuario
from sqlalchemy import select

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/auth/login")


async def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: AsyncSession = Depends(get_db),
) -> Usuario:
    """Dependencia que extrae y valida el usuario del token JWT."""
    credenciales_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="No se pudieron validar las credenciales",
        headers={"WWW-Authenticate": "Bearer"},
    )

    payload = verificar_token(token)
    if payload is None:
        raise credenciales_exception

    user_id: int | None = payload.get("sub")
    if user_id is None:
        raise credenciales_exception

    resultado = await db.execute(
        select(Usuario).where(Usuario.id == int(user_id))
    )
    usuario = resultado.scalar_one_or_none()

    if usuario is None or not usuario.activo:
        raise credenciales_exception

    return usuario
```

## 8.5 Router de Autenticación

```python
# app/routers/auth.py
from fastapi import APIRouter, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm
from sqlalchemy.ext.asyncio import AsyncSession
from app.dependencies import get_db
from app.core.security import verify_password, hash_password
from app.core.jwt import crear_token_acceso
from app.models.usuario import Usuario
from app.schemas.auth import TokenRespuesta, RegistroUsuario
from sqlalchemy import select

router = APIRouter(prefix="/auth", tags=["Autenticación"])


@router.post("/login", response_model=TokenRespuesta)
async def login(
    form_data: OAuth2PasswordRequestForm = Depends(),
    db: AsyncSession = Depends(get_db),
):
    """Autentica un usuario y retorna un token JWT."""
    resultado = await db.execute(
        select(Usuario).where(Usuario.email == form_data.username)
    )
    usuario = resultado.scalar_one_or_none()

    if not usuario or not verify_password(form_data.password, usuario.password_hash):
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Credenciales incorrectas",
            headers={"WWW-Authenticate": "Bearer"},
        )

    token = crear_token_acceso(datos={"sub": str(usuario.id)})
    return {"access_token": token, "token_type": "bearer"}


@router.post("/registro", status_code=201)
async def registro(
    datos: RegistroUsuario,
    db: AsyncSession = Depends(get_db),
):
    """Registra un nuevo usuario en el sistema."""
    existente = await db.execute(
        select(Usuario).where(Usuario.email == datos.email)
    )
    if existente.scalar_one_or_none():
        raise HTTPException(status_code=409, detail="El email ya está registrado")

    nuevo_usuario = Usuario(
        nombre=datos.nombre,
        email=datos.email,
        password_hash=hash_password(datos.password),
    )
    db.add(nuevo_usuario)
    await db.commit()
    return {"mensaje": "Usuario registrado exitosamente"}
```

## 8.6 Roles y Permisos

```python
# app/core/permissions.py
from enum import Enum
from functools import wraps
from fastapi import Depends, HTTPException, status
from app.core.auth import get_current_user
from app.models.usuario import Usuario


class Role(str, Enum):
    ADMIN = "admin"
    EDITOR = "editor"
    VIEWER = "viewer"


def require_roles(*roles: Role):
    """Dependencia que verifica que el usuario tenga uno de los roles requeridos."""
    async def verificar_rol(
        usuario: Usuario = Depends(get_current_user),
    ) -> Usuario:
        if usuario.rol not in roles:
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail="No tiene permisos suficientes",
            )
        return usuario
    return verificar_rol


# Uso en un endpoint
# @router.delete("/{id}", dependencies=[Depends(require_roles(Role.ADMIN))])
# async def eliminar_recurso(id: int):
#     ...
```

## 8.7 Protección de Endpoints

```python
from fastapi import APIRouter, Depends
from app.core.auth import get_current_user
from app.core.permissions import require_roles, Role
from app.models.usuario import Usuario

router = APIRouter(prefix="/admin", tags=["Administración"])


# Endpoint protegido: requiere autenticación
@router.get("/perfil")
async def mi_perfil(usuario: Usuario = Depends(get_current_user)):
    return {"id": usuario.id, "nombre": usuario.nombre, "email": usuario.email}


# Endpoint protegido: requiere rol específico
@router.get(
    "/dashboard",
    dependencies=[Depends(require_roles(Role.ADMIN))],
)
async def dashboard_admin():
    return {"mensaje": "Panel de administración"}
```

### Ejercicio Práctico

> Implementar un sistema completo de autenticación con: registro, login, endpoint protegido `/me`, y refresh token.

### Reto Opcional

> Implementar autenticación con OAuth2 usando un proveedor externo (Google o GitHub) además de la autenticación local con JWT.

### Resumen del Capítulo

La seguridad en FastAPI se implementa con JWT para autenticación stateless, bcrypt para hashing de contraseñas y OAuth2 como esquema estándar. El sistema de dependencias permite proteger endpoints de forma declarativa. Los roles y permisos se gestionan mediante dependencias reutilizables que verifican el nivel de acceso del usuario autenticado.

---

# 9. Programación Asíncrona

## 9.1 Fundamentos de async/await

Python soporta programación asíncrona de forma nativa mediante `asyncio`. FastAPI aprovecha este modelo para manejar miles de peticiones concurrentes sin bloquear el event loop.

### Diferencia entre síncrono y asíncrono

```python
import asyncio
import time

# --- SÍNCRONO ---
def tarea_sincrona():
    """Bloquea el hilo durante 2 segundos."""
    time.sleep(2)
    return "completado"


# --- ASÍNCRONO ---
async def tarea_asincrona():
    """No bloquea el event loop durante 2 segundos."""
    await asyncio.sleep(2)
    return "completado"


# Ejecutar múltiples tareas asíncronas de forma concurrente
async def main():
    # Ejecuta 3 tareas de 2 segundos en ~2 segundos (no en 6)
    resultados = await asyncio.gather(
        tarea_asincrona(),
        tarea_asincrona(),
        tarea_asincrona(),
    )
    print(resultados)  # ["completado", "completado", "completado"]
```

### Cuándo usar async en FastAPI

| Situación | Usar `async def` | Usar `def` |
|---|---|---|
| Llamadas a APIs externas (httpx) | ✅ | ❌ |
| Consultas async a base de datos | ✅ | ❌ |
| Lectura/escritura async de archivos | ✅ | ❌ |
| Operaciones CPU-intensivas | ❌ | ✅ |
| Librerías síncronas (requests) | ❌ | ✅ |

> **Regla:** Si la función usa `await`, debe ser `async def`. Si usa librerías síncronas bloqueantes, usar `def` (FastAPI las ejecutará en un threadpool).

## 9.2 Concurrencia en FastAPI

```python
import httpx
from fastapi import FastAPI

app = FastAPI()


@app.get("/datos-externos")
async def obtener_datos_externos():
    """Realiza múltiples peticiones HTTP de forma concurrente."""
    async with httpx.AsyncClient() as client:
        # Ambas peticiones se ejecutan simultáneamente
        respuesta_usuarios, respuesta_posts = await asyncio.gather(
            client.get("https://jsonplaceholder.typicode.com/users"),
            client.get("https://jsonplaceholder.typicode.com/posts"),
        )

    return {
        "usuarios": respuesta_usuarios.json()[:5],
        "posts": respuesta_posts.json()[:5],
    }
```

## 9.3 Background Tasks

Las tareas en segundo plano permiten ejecutar operaciones después de enviar la respuesta al cliente.

```python
import logging
from fastapi import FastAPI, BackgroundTasks
from pydantic import BaseModel, EmailStr

logger = logging.getLogger(__name__)
app = FastAPI()


def enviar_email_bienvenida(email: str, nombre: str):
    """Simula el envío de un email (operación lenta)."""
    import time
    time.sleep(5)  # Simula latencia de servicio de email
    logger.info(f"Email de bienvenida enviado a {email}")


def registrar_actividad(usuario_id: int, accion: str):
    """Registra la actividad del usuario en un log."""
    logger.info(f"Usuario {usuario_id}: {accion}")


class UsuarioRegistro(BaseModel):
    nombre: str
    email: EmailStr


@app.post("/registro")
async def registrar_usuario(
    usuario: UsuarioRegistro,
    background_tasks: BackgroundTasks,
):
    """
    Registra un usuario y envía un email de bienvenida en segundo plano.
    La respuesta se envía inmediatamente sin esperar al email.
    """
    # Aquí se crearía el usuario en la base de datos
    usuario_id = 1

    # Agregar tareas que se ejecutarán DESPUÉS de enviar la respuesta
    background_tasks.add_task(
        enviar_email_bienvenida, usuario.email, usuario.nombre
    )
    background_tasks.add_task(
        registrar_actividad, usuario_id, "registro"
    )

    return {"mensaje": "Usuario registrado", "id": usuario_id}
```

## 9.4 WebSockets

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from dataclasses import dataclass, field

app = FastAPI()


@dataclass
class ConnectionManager:
    """Gestiona las conexiones WebSocket activas."""
    conexiones_activas: list[WebSocket] = field(default_factory=list)

    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.conexiones_activas.append(websocket)

    def disconnect(self, websocket: WebSocket):
        self.conexiones_activas.remove(websocket)

    async def broadcast(self, mensaje: str):
        """Envía un mensaje a todas las conexiones activas."""
        for conexion in self.conexiones_activas:
            await conexion.send_text(mensaje)

    async def send_personal(self, mensaje: str, websocket: WebSocket):
        await websocket.send_text(mensaje)


manager = ConnectionManager()


@app.websocket("/ws/{cliente_id}")
async def websocket_endpoint(websocket: WebSocket, cliente_id: str):
    await manager.connect(websocket)
    await manager.broadcast(f"Cliente {cliente_id} se conectó")

    try:
        while True:
            datos = await websocket.receive_text()
            await manager.broadcast(f"{cliente_id}: {datos}")
    except WebSocketDisconnect:
        manager.disconnect(websocket)
        await manager.broadcast(f"Cliente {cliente_id} se desconectó")
```

### Cliente HTML para WebSocket

```html
<!DOCTYPE html>
<html>
<body>
    <h1>Chat WebSocket</h1>
    <input type="text" id="mensaje" placeholder="Escribe un mensaje...">
    <button onclick="enviar()">Enviar</button>
    <ul id="mensajes"></ul>

    <script>
        const clienteId = "usuario_" + Math.random().toString(36).substr(2, 5);
        const ws = new WebSocket(`ws://localhost:8000/ws/${clienteId}`);

        ws.onmessage = function(event) {
            const li = document.createElement("li");
            li.textContent = event.data;
            document.getElementById("mensajes").appendChild(li);
        };

        function enviar() {
            const input = document.getElementById("mensaje");
            ws.send(input.value);
            input.value = "";
        }
    </script>
</body>
</html>
```

### Diagrama de comunicación WebSocket

```mermaid
sequenceDiagram
    participant C1 as Cliente 1
    participant S as Servidor FastAPI
    participant C2 as Cliente 2

    C1->>S: Conexión WebSocket
    S-->>C1: Conexión aceptada
    C2->>S: Conexión WebSocket
    S-->>C2: Conexión aceptada
    S-->>C1: "Cliente 2 se conectó"

    C1->>S: "Hola a todos"
    S-->>C1: "Cliente1: Hola a todos"
    S-->>C2: "Cliente1: Hola a todos"

    C2->>S: Desconexión
    S-->>C1: "Cliente 2 se desconectó"
```

### Ejercicio Práctico

> Crear un sistema de notificaciones en tiempo real que utilice WebSockets para enviar actualizaciones a los clientes cuando se crea un nuevo recurso en la API (por ejemplo, un nuevo pedido).

### Reto Opcional

> Implementar un sistema de salas (rooms) donde los clientes solo reciban mensajes de la sala a la que están suscritos, con soporte para unirse, abandonar y listar salas.

### Resumen del Capítulo

La programación asíncrona es una de las fortalezas principales de FastAPI. Las funciones `async def` permiten manejar operaciones I/O de forma concurrente sin bloquear el servidor. Las Background Tasks ejecutan operaciones costosas después de enviar la respuesta. Los WebSockets permiten comunicación bidireccional en tiempo real entre clientes y servidor.

---

# 10. Testing

## 10.1 Configuración del Entorno de Testing

```bash
pip install pytest pytest-asyncio httpx
```

### Estructura de tests

```
tests/
├── __init__.py
├── conftest.py          # Fixtures compartidas
├── test_usuarios.py
├── test_auth.py
└── test_productos.py
```

## 10.2 TestClient y Pytest

```python
# tests/conftest.py
import pytest
from httpx import AsyncClient, ASGITransport
from sqlalchemy.ext.asyncio import (
    AsyncSession,
    async_sessionmaker,
    create_async_engine,
)
from app.main import app
from app.database import Base, get_db

# Base de datos de testing (SQLite en memoria)
TEST_DATABASE_URL = "sqlite+aiosqlite:///:memory:"

engine_test = create_async_engine(TEST_DATABASE_URL, echo=True)
async_session_test = async_sessionmaker(
    engine_test, class_=AsyncSession, expire_on_commit=False
)


async def override_get_db():
    async with async_session_test() as session:
        yield session


# Sobrescribir la dependencia de base de datos
app.dependency_overrides[get_db] = override_get_db


@pytest.fixture(autouse=True)
async def setup_database():
    """Crea y destruye las tablas para cada test."""
    async with engine_test.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    yield
    async with engine_test.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)


@pytest.fixture
async def client():
    """Provee un cliente HTTP async para testing."""
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as ac:
        yield ac


@pytest.fixture
async def db_session():
    """Provee una sesión de base de datos para testing."""
    async with async_session_test() as session:
        yield session
```

## 10.3 Tests de Endpoints

```python
# tests/test_usuarios.py
import pytest
from httpx import AsyncClient


@pytest.mark.asyncio
async def test_crear_usuario(client: AsyncClient):
    """Test: Crear un usuario retorna 201 y los datos correctos."""
    datos = {
        "nombre": "Ana García",
        "email": "ana@test.com",
        "password": "password123",
    }

    response = await client.post("/usuarios", json=datos)

    assert response.status_code == 201
    data = response.json()
    assert data["nombre"] == "Ana García"
    assert data["email"] == "ana@test.com"
    assert "id" in data
    assert "password" not in data  # El password no debe exponerse


@pytest.mark.asyncio
async def test_crear_usuario_email_duplicado(client: AsyncClient):
    """Test: Crear un usuario con email duplicado retorna 409."""
    datos = {
        "nombre": "Ana García",
        "email": "ana@test.com",
        "password": "password123",
    }

    await client.post("/usuarios", json=datos)
    response = await client.post("/usuarios", json=datos)

    assert response.status_code == 409


@pytest.mark.asyncio
async def test_obtener_usuario_no_existente(client: AsyncClient):
    """Test: Obtener un usuario inexistente retorna 404."""
    response = await client.get("/usuarios/999")
    assert response.status_code == 404


@pytest.mark.asyncio
async def test_listar_usuarios_paginacion(client: AsyncClient):
    """Test: La paginación funciona correctamente."""
    # Crear 5 usuarios
    for i in range(5):
        await client.post("/usuarios", json={
            "nombre": f"Usuario {i}",
            "email": f"user{i}@test.com",
            "password": "password123",
        })

    response = await client.get("/usuarios?skip=0&limit=3")
    assert response.status_code == 200
    assert len(response.json()) == 3
```

## 10.4 Tests de Autenticación

```python
# tests/test_auth.py
import pytest
from httpx import AsyncClient


@pytest.fixture
async def usuario_autenticado(client: AsyncClient) -> dict:
    """Fixture que registra un usuario y retorna el token."""
    await client.post("/auth/registro", json={
        "nombre": "Test User",
        "email": "test@test.com",
        "password": "password123",
    })

    response = await client.post("/auth/login", data={
        "username": "test@test.com",
        "password": "password123",
    })

    return response.json()


@pytest.mark.asyncio
async def test_login_exitoso(usuario_autenticado: dict):
    assert "access_token" in usuario_autenticado
    assert usuario_autenticado["token_type"] == "bearer"


@pytest.mark.asyncio
async def test_acceso_endpoint_protegido(
    client: AsyncClient,
    usuario_autenticado: dict,
):
    token = usuario_autenticado["access_token"]
    response = await client.get(
        "/admin/perfil",
        headers={"Authorization": f"Bearer {token}"},
    )
    assert response.status_code == 200


@pytest.mark.asyncio
async def test_acceso_sin_token(client: AsyncClient):
    response = await client.get("/admin/perfil")
    assert response.status_code == 401
```

## 10.5 Mocking

```python
# tests/test_con_mocking.py
import pytest
from unittest.mock import AsyncMock, patch
from httpx import AsyncClient


@pytest.mark.asyncio
async def test_enviar_email_mock(client: AsyncClient):
    """Test con mock del servicio de email."""
    with patch(
        "app.services.email.enviar_email",
        new_callable=AsyncMock,
    ) as mock_email:
        mock_email.return_value = True

        response = await client.post("/registro", json={
            "nombre": "Test",
            "email": "test@test.com",
            "password": "password123",
        })

        assert response.status_code == 201
        mock_email.assert_called_once_with(
            "test@test.com", "Bienvenido"
        )
```

## 10.6 Cobertura de Código

```bash
# Instalar plugin de cobertura
pip install pytest-cov

# Ejecutar tests con reporte de cobertura
pytest --cov=app --cov-report=html --cov-report=term-missing

# Ver el reporte HTML
open htmlcov/index.html
```

### Configuración en pyproject.toml

```toml
[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]
python_files = ["test_*.py"]
python_functions = ["test_*"]

[tool.coverage.run]
source = ["app"]
omit = ["app/main.py", "*/tests/*"]

[tool.coverage.report]
fail_under = 80
show_missing = true
```

### Ejercicio Práctico

> Escribir tests para una API de productos que incluya: creación, lectura, actualización, eliminación y validación de datos inválidos. Alcanzar al menos un 80% de cobertura.

### Reto Opcional

> Implementar tests de integración que verifiquen el flujo completo: registro → login → crear recurso → listar recursos → eliminar recurso, usando fixtures encadenadas.

### Resumen del Capítulo

El testing en FastAPI se realiza con pytest y httpx. Las fixtures permiten configurar el entorno de testing con bases de datos en memoria y clientes HTTP asíncronos. El mocking permite aislar componentes para tests unitarios. La cobertura de código garantiza que se alcance un nivel mínimo de testing en todo el proyecto.

---

# 11. Deployment

## 11.1 Dockerfile

```dockerfile
# Dockerfile
FROM python:3.12-slim AS base

# Variables de entorno
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Instalar dependencias del sistema
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Copiar e instalar dependencias de Python
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copiar el código fuente
COPY ./app ./app
COPY ./alembic ./alembic
COPY alembic.ini .

# Exponer el puerto
EXPOSE 8000

# Comando de inicio
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Multi-stage build para producción

```dockerfile
# Dockerfile.prod
FROM python:3.12-slim AS builder

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.12-slim AS production

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Copiar solo las dependencias instaladas
COPY --from=builder /install /usr/local

# Copiar código fuente
COPY ./app ./app
COPY ./alembic ./alembic
COPY alembic.ini .

# Crear usuario no-root
RUN addgroup --system appgroup && \
    adduser --system --ingroup appgroup appuser
USER appuser

EXPOSE 8000

CMD ["gunicorn", "app.main:app", \
     "--workers", "4", \
     "--worker-class", "uvicorn.workers.UvicornWorker", \
     "--bind", "0.0.0.0:8000", \
     "--access-logfile", "-"]
```

## 11.2 Docker Compose

```yaml
# docker-compose.yml
services:
  api:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql+asyncpg://postgres:postgres@db:5432/mi_api
      - SECRET_KEY=super-secret-key-cambiar-en-produccion
      - DEBUG=false
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: mi_api
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    restart: unless-stopped

volumes:
  postgres_data:
```

### Comandos útiles

```bash
# Construir y levantar todos los servicios
docker compose up --build -d

# Ver logs en tiempo real
docker compose logs -f api

# Ejecutar migraciones dentro del contenedor
docker compose exec api alembic upgrade head

# Acceder al shell del contenedor
docker compose exec api bash

# Detener todos los servicios
docker compose down

# Detener y eliminar volúmenes (CUIDADO: borra datos)
docker compose down -v
```

## 11.3 Gunicorn como Process Manager

En producción, Uvicorn debe ejecutarse detrás de Gunicorn para gestionar múltiples workers.

```bash
pip install gunicorn
```

```bash
# Ejecutar con Gunicorn + workers Uvicorn
gunicorn app.main:app \
    --workers 4 \
    --worker-class uvicorn.workers.UvicornWorker \
    --bind 0.0.0.0:8000 \
    --access-logfile - \
    --error-logfile - \
    --timeout 120
```

### Cálculo del número de workers

```python
# Fórmula recomendada:
# workers = (2 * CPU_cores) + 1

import multiprocessing
workers = (2 * multiprocessing.cpu_count()) + 1
```

## 11.4 Nginx como Reverse Proxy

```nginx
# nginx.conf
upstream api_backend {
    server api:8000;
}

server {
    listen 80;
    server_name api.midominio.com;

    # Redirigir HTTP a HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name api.midominio.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    # Seguridad
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";

    # Límites
    client_max_body_size 10M;

    location / {
        proxy_pass http://api_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # WebSockets
    location /ws {
        proxy_pass http://api_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

## 11.5 Variables de Entorno

```bash
# .env (desarrollo)
APP_NAME=Mi API
DEBUG=true
DATABASE_URL=postgresql+asyncpg://postgres:postgres@localhost:5432/mi_api
SECRET_KEY=dev-secret-key-no-usar-en-produccion
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

```bash
# .env.production
APP_NAME=Mi API
DEBUG=false
DATABASE_URL=postgresql+asyncpg://user:password@db-host:5432/produccion
SECRET_KEY=clave-segura-generada-aleatoriamente-de-64-caracteres
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=15
```

> **Importante:** Nunca subir archivos `.env` al repositorio. Usar servicios de gestión de secretos (AWS Secrets Manager, HashiCorp Vault) en producción.

## 11.6 Checklist de Producción

| Elemento | Estado |
|---|---|
| Variables de entorno configuradas | ☐ |
| `DEBUG=false` | ☐ |
| Secret key segura y única | ☐ |
| HTTPS habilitado | ☐ |
| CORS configurado correctamente | ☐ |
| Documentación deshabilitada (o protegida) | ☐ |
| Rate limiting configurado | ☐ |
| Logging estructurado | ☐ |
| Health check endpoint | ☐ |
| Migraciones ejecutadas | ☐ |
| Backups de base de datos | ☐ |
| Monitoreo y alertas | ☐ |

### Ejercicio Práctico

> Dockerizar la API de tareas creada en los capítulos anteriores. Configurar docker-compose con PostgreSQL y ejecutar las migraciones automáticamente al iniciar el contenedor.

### Reto Opcional

> Configurar un pipeline CI/CD con GitHub Actions que ejecute los tests, construya la imagen Docker y la publique en un registry (Docker Hub o GitHub Container Registry).

### Resumen del Capítulo

El despliegue profesional de una API FastAPI requiere Docker para la contenedorización, Gunicorn como gestor de procesos con workers Uvicorn, y Nginx como reverse proxy con SSL. Docker Compose orquesta los servicios (API, base de datos, cache). Las variables de entorno gestionan la configuración específica de cada ambiente.

---

# 12. Buenas Prácticas

## 12.1 Clean Architecture

La Clean Architecture separa el código en capas con dependencias unidireccionales (de afuera hacia adentro):

```mermaid
graph TD
    subgraph Capa Externa
        A[Routers / Controllers]
        B[Base de Datos]
        C[APIs Externas]
    end

    subgraph Capa de Aplicación
        D[Services / Use Cases]
    end

    subgraph Capa de Dominio
        E[Entidades]
        F[Interfaces / Puertos]
    end

    A --> D
    D --> E
    D --> F
    B -.-> F
    C -.-> F
```

### Principio de inversión de dependencias aplicado

```python
# app/ports/usuario_repository.py
from abc import ABC, abstractmethod
from app.schemas.usuario import UsuarioCrear


class UsuarioRepositoryPort(ABC):
    """Interfaz que define el contrato de acceso a datos de usuario."""

    @abstractmethod
    async def crear(self, datos: UsuarioCrear) -> dict:
        ...

    @abstractmethod
    async def obtener_por_id(self, usuario_id: int) -> dict | None:
        ...

    @abstractmethod
    async def obtener_por_email(self, email: str) -> dict | None:
        ...
```

```python
# app/adapters/sqlalchemy_usuario_repository.py
from app.ports.usuario_repository import UsuarioRepositoryPort
from app.schemas.usuario import UsuarioCrear
from sqlalchemy.ext.asyncio import AsyncSession


class SQLAlchemyUsuarioRepository(UsuarioRepositoryPort):
    """Implementación concreta usando SQLAlchemy."""

    def __init__(self, db: AsyncSession):
        self.db = db

    async def crear(self, datos: UsuarioCrear) -> dict:
        # Implementación con SQLAlchemy
        ...

    async def obtener_por_id(self, usuario_id: int) -> dict | None:
        # Implementación con SQLAlchemy
        ...

    async def obtener_por_email(self, email: str) -> dict | None:
        # Implementación con SQLAlchemy
        ...
```

## 12.2 Principios SOLID

| Principio | Aplicación en FastAPI |
|---|---|
| **S** - Single Responsibility | Cada router, service y repository tiene una única responsabilidad |
| **O** - Open/Closed | Los modelos Pydantic se extienden por herencia sin modificar los existentes |
| **L** - Liskov Substitution | Las implementaciones de repositorios son intercambiables |
| **I** - Interface Segregation | Interfaces específicas por dominio, no interfaces genéricas |
| **D** - Dependency Inversion | Las capas superiores dependen de abstracciones (puertos), no de implementaciones |

## 12.3 Manejo de Errores

```python
# app/exceptions.py
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse
import logging

logger = logging.getLogger(__name__)


class AppException(Exception):
    """Excepción base de la aplicación."""
    def __init__(
        self,
        status_code: int = 500,
        detail: str = "Error interno del servidor",
        error_code: str = "INTERNAL_ERROR",
    ):
        self.status_code = status_code
        self.detail = detail
        self.error_code = error_code


class EntityNotFound(AppException):
    def __init__(self, entity: str, entity_id: int | str):
        super().__init__(
            status_code=404,
            detail=f"{entity} con ID '{entity_id}' no encontrado",
            error_code="NOT_FOUND",
        )


class BusinessRuleViolation(AppException):
    def __init__(self, detail: str):
        super().__init__(
            status_code=422,
            detail=detail,
            error_code="BUSINESS_RULE_VIOLATION",
        )


def register_exception_handlers(app: FastAPI):
    """Registra los manejadores globales de excepciones."""

    @app.exception_handler(AppException)
    async def app_exception_handler(request: Request, exc: AppException):
        logger.warning(f"{exc.error_code}: {exc.detail} - {request.url}")
        return JSONResponse(
            status_code=exc.status_code,
            content={
                "error": exc.error_code,
                "detail": exc.detail,
            },
        )

    @app.exception_handler(Exception)
    async def generic_exception_handler(request: Request, exc: Exception):
        logger.error(f"Error no manejado: {exc}", exc_info=True)
        return JSONResponse(
            status_code=500,
            content={
                "error": "INTERNAL_ERROR",
                "detail": "Ocurrió un error interno",
            },
        )
```

## 12.4 Logging Estructurado

```python
# app/logging_config.py
import logging
import sys
from app.config import get_settings


def setup_logging():
    """Configura el logging de la aplicación."""
    settings = get_settings()

    level = logging.DEBUG if settings.debug else logging.INFO

    logging.basicConfig(
        level=level,
        format="%(asctime)s | %(levelname)-8s | %(name)s | %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        handlers=[logging.StreamHandler(sys.stdout)],
    )

    # Silenciar logs excesivos de librerías externas
    logging.getLogger("uvicorn.access").setLevel(logging.WARNING)
    logging.getLogger("sqlalchemy.engine").setLevel(
        logging.DEBUG if settings.debug else logging.WARNING
    )
```

### Uso en la aplicación

```python
import logging

logger = logging.getLogger(__name__)


class UsuarioService:
    async def crear(self, datos):
        logger.info(f"Creando usuario: {datos.email}")
        try:
            usuario = await self.repositorio.crear(datos)
            logger.info(f"Usuario creado exitosamente: ID={usuario.id}")
            return usuario
        except Exception as e:
            logger.error(f"Error al crear usuario: {e}", exc_info=True)
            raise
```

## 12.5 Configuración por Ambiente

```python
# app/config.py
from pydantic_settings import BaseSettings
from functools import lru_cache


class BaseConfig(BaseSettings):
    app_name: str = "Mi API"
    debug: bool = False
    database_url: str
    secret_key: str

    model_config = {"env_file": ".env"}


class DevelopmentConfig(BaseConfig):
    debug: bool = True
    model_config = {"env_file": ".env.development"}


class ProductionConfig(BaseConfig):
    debug: bool = False
    model_config = {"env_file": ".env.production"}


class TestingConfig(BaseConfig):
    debug: bool = True
    database_url: str = "sqlite+aiosqlite:///:memory:"
    secret_key: str = "test-secret"


@lru_cache
def get_settings() -> BaseConfig:
    import os
    env = os.getenv("ENVIRONMENT", "development")
    configs = {
        "development": DevelopmentConfig,
        "production": ProductionConfig,
        "testing": TestingConfig,
    }
    return configs[env]()
```

## 12.6 Escalabilidad

### Patrones recomendados

1. **Caché con Redis** para respuestas frecuentes.
2. **Rate Limiting** para proteger la API de abuso.
3. **Paginación cursor-based** para grandes conjuntos de datos.
4. **Procesamiento asíncrono** con colas de tareas (Celery, Redis Queue).
5. **Health checks** para orquestadores (Kubernetes, Docker Swarm).

```python
# Ejemplo: Health check completo
from fastapi import FastAPI
from sqlalchemy import text
from app.database import async_session_maker

app = FastAPI()


@app.get("/health")
async def health_check():
    """Verifica el estado de todos los servicios."""
    checks = {"api": "ok"}

    try:
        async with async_session_maker() as session:
            await session.execute(text("SELECT 1"))
            checks["database"] = "ok"
    except Exception:
        checks["database"] = "error"

    status = "healthy" if all(v == "ok" for v in checks.values()) else "unhealthy"
    return {"status": status, "checks": checks}
```

### Ejercicio Práctico

> Implementar un sistema de manejo de errores centralizado con excepciones personalizadas, logging estructurado y respuestas de error consistentes en toda la API.

### Reto Opcional

> Implementar caché con Redis usando decoradores personalizados que invaliden el caché automáticamente cuando se modifiquen los datos subyacentes.

### Resumen del Capítulo

Las buenas prácticas en FastAPI incluyen aplicar Clean Architecture y principios SOLID para mantener el código organizado y testeable. El manejo centralizado de errores y el logging estructurado son esenciales para la observabilidad en producción. La configuración por ambiente permite adaptar el comportamiento de la aplicación según el entorno de ejecución. Los patrones de escalabilidad preparan la API para manejar cargas de trabajo crecientes.

---

# 13. Middleware, CORS y Eventos del Ciclo de Vida

## 13.1 Middleware

Un middleware es una función que se ejecuta con **cada petición** antes de ser procesada por el endpoint, y con **cada respuesta** antes de ser enviada al cliente.

```mermaid
graph LR
    A[Cliente] --> B[Middleware 1]
    B --> C[Middleware 2]
    C --> D[Endpoint]
    D --> C
    C --> B
    B --> A
```

### Middleware personalizado

```python
import time
import logging
from fastapi import FastAPI, Request

logger = logging.getLogger(__name__)
app = FastAPI()


@app.middleware("http")
async def log_requests(request: Request, call_next):
    """Middleware que registra el tiempo de cada petición."""
    start_time = time.perf_counter()
    request_id = request.headers.get("X-Request-ID", "N/A")

    logger.info(f"[{request_id}] {request.method} {request.url.path} - Iniciando")

    response = await call_next(request)

    duration = time.perf_counter() - start_time
    response.headers["X-Process-Time"] = f"{duration:.4f}"
    logger.info(
        f"[{request_id}] {request.method} {request.url.path} "
        f"- {response.status_code} - {duration:.4f}s"
    )

    return response
```

### Middleware de clase (Starlette)

```python
from starlette.middleware.base import BaseHTTPMiddleware
from starlette.requests import Request
from starlette.responses import Response


class RateLimitMiddleware(BaseHTTPMiddleware):
    """Middleware de rate limiting básico usando un diccionario en memoria."""

    def __init__(self, app, max_requests: int = 100, window_seconds: int = 60):
        super().__init__(app)
        self.max_requests = max_requests
        self.window_seconds = window_seconds
        self.requests: dict[str, list[float]] = {}

    async def dispatch(self, request: Request, call_next) -> Response:
        client_ip = request.client.host
        current_time = time.time()

        if client_ip not in self.requests:
            self.requests[client_ip] = []

        # Limpiar peticiones fuera de la ventana
        self.requests[client_ip] = [
            t for t in self.requests[client_ip]
            if current_time - t < self.window_seconds
        ]

        if len(self.requests[client_ip]) >= self.max_requests:
            return JSONResponse(
                status_code=429,
                content={"detail": "Demasiadas peticiones"},
            )

        self.requests[client_ip].append(current_time)
        return await call_next(request)


# Registrar middleware
app.add_middleware(RateLimitMiddleware, max_requests=100, window_seconds=60)
```

## 13.2 CORS (Cross-Origin Resource Sharing)

CORS es esencial cuando el frontend y el backend están en dominios o puertos diferentes.

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Configuración de CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:3000",          # Frontend React/Vue en desarrollo
        "https://miapp.com",              # Frontend en producción
    ],
    allow_credentials=True,               # Permite enviar cookies
    allow_methods=["GET", "POST", "PUT", "DELETE", "PATCH"],
    allow_headers=["*"],                  # Permite todos los headers
    expose_headers=["X-Total-Count"],     # Headers visibles para el cliente
    max_age=600,                          # Cache de preflight en segundos
)
```

| Parámetro | Descripción |
|---|---|
| `allow_origins` | Lista de orígenes permitidos. Usar `["*"]` solo en desarrollo |
| `allow_credentials` | Permite el envío de cookies cross-origin |
| `allow_methods` | Métodos HTTP permitidos |
| `allow_headers` | Headers de petición permitidos |
| `expose_headers` | Headers de respuesta accesibles desde JavaScript |
| `max_age` | Tiempo de cache del preflight en segundos |

## 13.3 Eventos del Ciclo de Vida

FastAPI permite ejecutar código al iniciar y al cerrar la aplicación usando `lifespan`.

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI
from app.database import init_db, engine
from app.logging_config import setup_logging
import logging

logger = logging.getLogger(__name__)


@asynccontextmanager
async def lifespan(app: FastAPI):
    """Gestiona el ciclo de vida de la aplicación."""
    # --- STARTUP ---
    setup_logging()
    logger.info("Iniciando aplicación...")
    await init_db()
    logger.info("Base de datos inicializada")
    # Aquí se pueden inicializar: conexiones Redis, clientes HTTP, etc.

    yield  # La aplicación se ejecuta aquí

    # --- SHUTDOWN ---
    logger.info("Cerrando aplicación...")
    await engine.dispose()
    logger.info("Conexiones de base de datos cerradas")


app = FastAPI(lifespan=lifespan)
```

### Ejercicio Práctico

> Implementar tres middlewares: uno de logging de peticiones, uno de manejo de CORS y uno que añada un `X-Request-ID` único a cada petición. Configurar el lifespan para inicializar y cerrar la conexión a base de datos.

### Reto Opcional

> Crear un middleware de caché que almacene respuestas GET en memoria durante un tiempo configurable, invalidando el caché cuando se reciban peticiones POST/PUT/DELETE al mismo recurso.

### Resumen del Capítulo

Los middlewares interceptan cada petición y respuesta, permitiendo implementar lógica transversal como logging, rate limiting y autenticación global. CORS es obligatorio cuando el frontend y backend están en orígenes diferentes. Los eventos de ciclo de vida (`lifespan`) permiten inicializar y liberar recursos de forma controlada al iniciar y cerrar la aplicación.

---

# 14. Proyecto Final: API de Gestión de Tareas Profesional

## 14.1 Descripción del Proyecto

Se construirá una **API REST completa** para gestión de tareas (Task Manager) con las siguientes características:

- CRUD completo de usuarios y tareas.
- Autenticación JWT con registro y login.
- Base de datos PostgreSQL con SQLAlchemy async.
- Arquitectura por capas (routers, services, repositories, schemas).
- Docker y Docker Compose.
- Tests automatizados.

### Diagrama de arquitectura

```mermaid
graph TD
    subgraph Docker Compose
        subgraph API["FastAPI Application"]
            R[Routers] --> S[Services]
            S --> RP[Repositories]
            RP --> DB_CONN[AsyncSession]
        end
        DB[(PostgreSQL)]
        DB_CONN --> DB
    end
    CLIENT[Cliente HTTP] --> R
```

## 14.2 Estructura del Proyecto

```
task-manager/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   ├── database.py
│   ├── dependencies.py
│   ├── exceptions.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── security.py
│   │   ├── jwt.py
│   │   └── auth.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   └── tarea.py
│   ├── schemas/
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   ├── tarea.py
│   │   └── auth.py
│   ├── routers/
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── usuarios.py
│   │   └── tareas.py
│   ├── services/
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   └── tarea.py
│   └── repositories/
│       ├── __init__.py
│       ├── usuario.py
│       └── tarea.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── test_auth.py
│   ├── test_usuarios.py
│   └── test_tareas.py
├── alembic/
│   └── versions/
├── alembic.ini
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── .env
├── .env.example
└── .gitignore
```

## 14.3 Configuración

```python
# app/config.py
from functools import lru_cache
from pydantic_settings import BaseSettings


class Settings(BaseSettings):
    app_name: str = "Task Manager API"
    debug: bool = False
    database_url: str = "postgresql+asyncpg://postgres:postgres@localhost:5432/tasks"
    secret_key: str = "cambiar-en-produccion"
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30

    model_config = {"env_file": ".env"}


@lru_cache
def get_settings() -> Settings:
    return Settings()
```

```python
# app/database.py
from sqlalchemy.ext.asyncio import (
    AsyncSession,
    async_sessionmaker,
    create_async_engine,
)
from sqlalchemy.orm import DeclarativeBase
from app.config import get_settings

settings = get_settings()

engine = create_async_engine(settings.database_url, echo=settings.debug)
async_session_maker = async_sessionmaker(
    engine, class_=AsyncSession, expire_on_commit=False
)


class Base(DeclarativeBase):
    pass


async def get_db():
    async with async_session_maker() as session:
        yield session
```

## 14.4 Modelos ORM

```python
# app/models/usuario.py
from datetime import datetime
from sqlalchemy import String, Boolean, DateTime, func
from sqlalchemy.orm import Mapped, mapped_column, relationship
from app.database import Base


class Usuario(Base):
    __tablename__ = "usuarios"

    id: Mapped[int] = mapped_column(primary_key=True, autoincrement=True)
    nombre: Mapped[str] = mapped_column(String(100))
    email: Mapped[str] = mapped_column(String(255), unique=True, index=True)
    password_hash: Mapped[str] = mapped_column(String(255))
    activo: Mapped[bool] = mapped_column(Boolean, default=True)
    creado_en: Mapped[datetime] = mapped_column(DateTime, server_default=func.now())

    tareas: Mapped[list["Tarea"]] = relationship(
        back_populates="propietario", cascade="all, delete-orphan"
    )
```

```python
# app/models/tarea.py
from datetime import datetime
from enum import Enum as PyEnum
from sqlalchemy import String, Text, ForeignKey, DateTime, Enum, func
from sqlalchemy.orm import Mapped, mapped_column, relationship
from app.database import Base


class PrioridadEnum(str, PyEnum):
    BAJA = "baja"
    MEDIA = "media"
    ALTA = "alta"
    CRITICA = "critica"


class EstadoEnum(str, PyEnum):
    PENDIENTE = "pendiente"
    EN_PROGRESO = "en_progreso"
    COMPLETADA = "completada"
    CANCELADA = "cancelada"


class Tarea(Base):
    __tablename__ = "tareas"

    id: Mapped[int] = mapped_column(primary_key=True, autoincrement=True)
    titulo: Mapped[str] = mapped_column(String(200))
    descripcion: Mapped[str | None] = mapped_column(Text, nullable=True)
    estado: Mapped[EstadoEnum] = mapped_column(
        Enum(EstadoEnum), default=EstadoEnum.PENDIENTE
    )
    prioridad: Mapped[PrioridadEnum] = mapped_column(
        Enum(PrioridadEnum), default=PrioridadEnum.MEDIA
    )
    propietario_id: Mapped[int] = mapped_column(ForeignKey("usuarios.id"))
    creado_en: Mapped[datetime] = mapped_column(DateTime, server_default=func.now())
    actualizado_en: Mapped[datetime] = mapped_column(
        DateTime, server_default=func.now(), onupdate=func.now()
    )

    propietario: Mapped["Usuario"] = relationship(back_populates="tareas")
```

## 14.5 Schemas Pydantic

```python
# app/schemas/auth.py
from pydantic import BaseModel, EmailStr, Field


class RegistroRequest(BaseModel):
    nombre: str = Field(..., min_length=2, max_length=100)
    email: EmailStr
    password: str = Field(..., min_length=8, max_length=128)


class LoginRequest(BaseModel):
    email: EmailStr
    password: str


class TokenResponse(BaseModel):
    access_token: str
    token_type: str = "bearer"
```

```python
# app/schemas/tarea.py
from datetime import datetime
from pydantic import BaseModel, Field, ConfigDict
from app.models.tarea import EstadoEnum, PrioridadEnum


class TareaCrear(BaseModel):
    titulo: str = Field(..., min_length=1, max_length=200)
    descripcion: str | None = Field(default=None, max_length=2000)
    prioridad: PrioridadEnum = PrioridadEnum.MEDIA


class TareaActualizar(BaseModel):
    titulo: str | None = Field(default=None, min_length=1, max_length=200)
    descripcion: str | None = None
    estado: EstadoEnum | None = None
    prioridad: PrioridadEnum | None = None


class TareaRespuesta(BaseModel):
    model_config = ConfigDict(from_attributes=True)

    id: int
    titulo: str
    descripcion: str | None
    estado: EstadoEnum
    prioridad: PrioridadEnum
    propietario_id: int
    creado_en: datetime
    actualizado_en: datetime
```

## 14.6 Repository

```python
# app/repositories/tarea.py
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select, update, delete
from app.models.tarea import Tarea, EstadoEnum
from app.schemas.tarea import TareaCrear, TareaActualizar


class TareaRepository:
    def __init__(self, db: AsyncSession):
        self.db = db

    async def crear(self, datos: TareaCrear, propietario_id: int) -> Tarea:
        tarea = Tarea(**datos.model_dump(), propietario_id=propietario_id)
        self.db.add(tarea)
        await self.db.commit()
        await self.db.refresh(tarea)
        return tarea

    async def obtener_por_id(self, tarea_id: int, propietario_id: int) -> Tarea | None:
        resultado = await self.db.execute(
            select(Tarea).where(
                Tarea.id == tarea_id,
                Tarea.propietario_id == propietario_id,
            )
        )
        return resultado.scalar_one_or_none()

    async def listar(
        self,
        propietario_id: int,
        estado: EstadoEnum | None = None,
        skip: int = 0,
        limit: int = 20,
    ) -> list[Tarea]:
        query = select(Tarea).where(Tarea.propietario_id == propietario_id)
        if estado:
            query = query.where(Tarea.estado == estado)
        query = query.offset(skip).limit(limit).order_by(Tarea.creado_en.desc())
        resultado = await self.db.execute(query)
        return list(resultado.scalars().all())

    async def actualizar(
        self, tarea_id: int, propietario_id: int, datos: TareaActualizar
    ) -> Tarea | None:
        valores = datos.model_dump(exclude_unset=True)
        if not valores:
            return await self.obtener_por_id(tarea_id, propietario_id)

        await self.db.execute(
            update(Tarea)
            .where(Tarea.id == tarea_id, Tarea.propietario_id == propietario_id)
            .values(**valores)
        )
        await self.db.commit()
        return await self.obtener_por_id(tarea_id, propietario_id)

    async def eliminar(self, tarea_id: int, propietario_id: int) -> bool:
        resultado = await self.db.execute(
            delete(Tarea).where(
                Tarea.id == tarea_id,
                Tarea.propietario_id == propietario_id,
            )
        )
        await self.db.commit()
        return resultado.rowcount > 0
```

## 14.7 Service

```python
# app/services/tarea.py
from app.repositories.tarea import TareaRepository
from app.schemas.tarea import TareaCrear, TareaActualizar, TareaRespuesta
from app.models.tarea import EstadoEnum
from app.exceptions import EntityNotFound
import logging

logger = logging.getLogger(__name__)


class TareaService:
    def __init__(self, repositorio: TareaRepository):
        self.repositorio = repositorio

    async def crear(self, datos: TareaCrear, propietario_id: int) -> TareaRespuesta:
        logger.info(f"Creando tarea para usuario {propietario_id}: {datos.titulo}")
        tarea = await self.repositorio.crear(datos, propietario_id)
        return TareaRespuesta.model_validate(tarea)

    async def obtener(self, tarea_id: int, propietario_id: int) -> TareaRespuesta:
        tarea = await self.repositorio.obtener_por_id(tarea_id, propietario_id)
        if not tarea:
            raise EntityNotFound("Tarea", tarea_id)
        return TareaRespuesta.model_validate(tarea)

    async def listar(
        self,
        propietario_id: int,
        estado: EstadoEnum | None = None,
        skip: int = 0,
        limit: int = 20,
    ) -> list[TareaRespuesta]:
        tareas = await self.repositorio.listar(propietario_id, estado, skip, limit)
        return [TareaRespuesta.model_validate(t) for t in tareas]

    async def actualizar(
        self, tarea_id: int, propietario_id: int, datos: TareaActualizar
    ) -> TareaRespuesta:
        tarea = await self.repositorio.actualizar(tarea_id, propietario_id, datos)
        if not tarea:
            raise EntityNotFound("Tarea", tarea_id)
        logger.info(f"Tarea {tarea_id} actualizada")
        return TareaRespuesta.model_validate(tarea)

    async def eliminar(self, tarea_id: int, propietario_id: int) -> None:
        eliminada = await self.repositorio.eliminar(tarea_id, propietario_id)
        if not eliminada:
            raise EntityNotFound("Tarea", tarea_id)
        logger.info(f"Tarea {tarea_id} eliminada")
```

## 14.8 Router

```python
# app/routers/tareas.py
from fastapi import APIRouter, Depends, Query, status
from app.schemas.tarea import TareaCrear, TareaActualizar, TareaRespuesta
from app.services.tarea import TareaService
from app.models.tarea import EstadoEnum
from app.models.usuario import Usuario
from app.core.auth import get_current_user
from app.dependencies import get_tarea_service

router = APIRouter(prefix="/tareas", tags=["Tareas"])


@router.post("/", response_model=TareaRespuesta, status_code=201)
async def crear_tarea(
    datos: TareaCrear,
    usuario: Usuario = Depends(get_current_user),
    servicio: TareaService = Depends(get_tarea_service),
):
    """Crea una nueva tarea para el usuario autenticado."""
    return await servicio.crear(datos, usuario.id)


@router.get("/", response_model=list[TareaRespuesta])
async def listar_tareas(
    estado: EstadoEnum | None = None,
    skip: int = Query(default=0, ge=0),
    limit: int = Query(default=20, ge=1, le=100),
    usuario: Usuario = Depends(get_current_user),
    servicio: TareaService = Depends(get_tarea_service),
):
    """Lista las tareas del usuario autenticado con filtros opcionales."""
    return await servicio.listar(usuario.id, estado, skip, limit)


@router.get("/{tarea_id}", response_model=TareaRespuesta)
async def obtener_tarea(
    tarea_id: int,
    usuario: Usuario = Depends(get_current_user),
    servicio: TareaService = Depends(get_tarea_service),
):
    """Obtiene una tarea por su ID."""
    return await servicio.obtener(tarea_id, usuario.id)


@router.patch("/{tarea_id}", response_model=TareaRespuesta)
async def actualizar_tarea(
    tarea_id: int,
    datos: TareaActualizar,
    usuario: Usuario = Depends(get_current_user),
    servicio: TareaService = Depends(get_tarea_service),
):
    """Actualiza parcialmente una tarea."""
    return await servicio.actualizar(tarea_id, usuario.id, datos)


@router.delete("/{tarea_id}", status_code=status.HTTP_204_NO_CONTENT)
async def eliminar_tarea(
    tarea_id: int,
    usuario: Usuario = Depends(get_current_user),
    servicio: TareaService = Depends(get_tarea_service),
):
    """Elimina una tarea."""
    await servicio.eliminar(tarea_id, usuario.id)
```

## 14.9 Main (Punto de Entrada)

```python
# app/main.py
from contextlib import asynccontextmanager
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from app.config import get_settings
from app.database import engine, Base
from app.routers import auth, usuarios, tareas
from app.exceptions import register_exception_handlers
import logging

logger = logging.getLogger(__name__)
settings = get_settings()


@asynccontextmanager
async def lifespan(app: FastAPI):
    logger.info(f"Iniciando {settings.app_name}...")
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    logger.info("Base de datos inicializada")
    yield
    await engine.dispose()
    logger.info("Aplicación cerrada")


app = FastAPI(
    title=settings.app_name,
    version="1.0.0",
    lifespan=lifespan,
)

# CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Excepciones globales
register_exception_handlers(app)

# Routers
app.include_router(auth.router)
app.include_router(usuarios.router)
app.include_router(tareas.router)


@app.get("/health")
async def health():
    return {"status": "healthy", "app": settings.app_name}
```

## 14.10 Docker

```dockerfile
# Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY ./app ./app
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql+asyncpg://postgres:postgres@db:5432/tasks
      - SECRET_KEY=dev-secret-key
      - DEBUG=true
    depends_on:
      db:
        condition: service_healthy
    volumes:
      - ./app:/app/app

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: tasks
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  pgdata:
```

## 14.11 Tests del Proyecto

```python
# tests/test_tareas.py
import pytest
from httpx import AsyncClient


@pytest.fixture
async def auth_headers(client: AsyncClient) -> dict:
    """Registra un usuario y retorna headers con token."""
    await client.post("/auth/registro", json={
        "nombre": "Test User",
        "email": "test@test.com",
        "password": "password123",
    })
    response = await client.post("/auth/login", data={
        "username": "test@test.com",
        "password": "password123",
    })
    token = response.json()["access_token"]
    return {"Authorization": f"Bearer {token}"}


@pytest.mark.asyncio
async def test_crear_tarea(client: AsyncClient, auth_headers: dict):
    response = await client.post(
        "/tareas/",
        json={"titulo": "Mi tarea", "prioridad": "alta"},
        headers=auth_headers,
    )
    assert response.status_code == 201
    data = response.json()
    assert data["titulo"] == "Mi tarea"
    assert data["estado"] == "pendiente"
    assert data["prioridad"] == "alta"


@pytest.mark.asyncio
async def test_listar_tareas(client: AsyncClient, auth_headers: dict):
    # Crear 3 tareas
    for i in range(3):
        await client.post(
            "/tareas/",
            json={"titulo": f"Tarea {i}"},
            headers=auth_headers,
        )

    response = await client.get("/tareas/", headers=auth_headers)
    assert response.status_code == 200
    assert len(response.json()) == 3


@pytest.mark.asyncio
async def test_actualizar_tarea(client: AsyncClient, auth_headers: dict):
    # Crear tarea
    resp = await client.post(
        "/tareas/",
        json={"titulo": "Original"},
        headers=auth_headers,
    )
    tarea_id = resp.json()["id"]

    # Actualizar
    response = await client.patch(
        f"/tareas/{tarea_id}",
        json={"titulo": "Actualizada", "estado": "en_progreso"},
        headers=auth_headers,
    )
    assert response.status_code == 200
    assert response.json()["titulo"] == "Actualizada"
    assert response.json()["estado"] == "en_progreso"


@pytest.mark.asyncio
async def test_eliminar_tarea(client: AsyncClient, auth_headers: dict):
    resp = await client.post(
        "/tareas/",
        json={"titulo": "A eliminar"},
        headers=auth_headers,
    )
    tarea_id = resp.json()["id"]

    response = await client.delete(f"/tareas/{tarea_id}", headers=auth_headers)
    assert response.status_code == 204

    response = await client.get(f"/tareas/{tarea_id}", headers=auth_headers)
    assert response.status_code == 404


@pytest.mark.asyncio
async def test_tarea_no_accesible_por_otro_usuario(client: AsyncClient, auth_headers: dict):
    """Verifica que un usuario no puede acceder a tareas de otro."""
    # Crear tarea con usuario 1
    resp = await client.post(
        "/tareas/",
        json={"titulo": "Tarea privada"},
        headers=auth_headers,
    )
    tarea_id = resp.json()["id"]

    # Registrar usuario 2
    await client.post("/auth/registro", json={
        "nombre": "Otro User",
        "email": "otro@test.com",
        "password": "password123",
    })
    resp2 = await client.post("/auth/login", data={
        "username": "otro@test.com",
        "password": "password123",
    })
    headers2 = {"Authorization": f"Bearer {resp2.json()['access_token']}"}

    # Intentar acceder a la tarea del usuario 1
    response = await client.get(f"/tareas/{tarea_id}", headers=headers2)
    assert response.status_code == 404
```

## 14.12 Ejecución

```bash
# Levantar con Docker Compose
docker compose up --build -d

# Verificar que funciona
curl http://localhost:8000/health

# Ejecutar tests
docker compose exec api pytest tests/ -v --cov=app

# Ver documentación
# Swagger: http://localhost:8000/docs
# ReDoc:   http://localhost:8000/redoc
```

### Resumen del Proyecto Final

Este proyecto integra todos los conceptos del curso: arquitectura por capas, autenticación JWT, base de datos asíncrona con SQLAlchemy, validación con Pydantic, testing automatizado y despliegue con Docker. La estructura es escalable y sirve como base para proyectos profesionales reales.

---

# Recursos Adicionales

| Recurso | Enlace |
|---|---|
| Documentación oficial de FastAPI | https://fastapi.tiangolo.com |
| Repositorio GitHub de FastAPI | https://github.com/tiangolo/fastapi |
| Documentación de Pydantic v2 | https://docs.pydantic.dev |
| Documentación de SQLAlchemy | https://docs.sqlalchemy.org |
| Documentación de Alembic | https://alembic.sqlalchemy.org |
| Documentación de Uvicorn | https://www.uvicorn.org |
| Documentación de Starlette | https://www.starlette.io |

---

> **Nota final:** Este curso cubre los fundamentos y aspectos avanzados de FastAPI. El aprendizaje continuo es esencial: se recomienda seguir la documentación oficial, contribuir a proyectos open source y construir proyectos reales para consolidar los conocimientos adquiridos.
