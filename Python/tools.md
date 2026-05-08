# Python Tools Documentation

## uv - Python Package Manager

`uv` es un gestor de paquetes y entornos virtuales de Python extremadamente rápido, escrito en Rust. Reemplaza herramientas como pip, pip-tools y virtualenv con un enfoque más moderno y eficiente.

### Instalación

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Con Homebrew (macOS)
brew install uv
```

### Comandos Principales

#### Gestión de Entornos

```bash
# Crear un entorno virtual
uv venv

# Crear un entorno con una versión específica de Python
uv venv --python 3.11

# Activar el entorno (macOS/Linux)
source .venv/bin/activate

# Activar el entorno (Windows)
.venv\Scripts\activate
```

#### Instalación de Paquetes

```bash
# Instalar paquetes en el entorno activo
uv pip install requests

# Instalar múltiples paquetes
uv pip install requests numpy pandas

# Instalar desde un archivo requirements.txt
uv pip install -r requirements.txt

# Instalar versión específica
uv pip install requests==2.28.0
```

#### Gestión de Dependencias

```bash
# Compilar requirements desde archivos .in (si usas pip-tools)
uv pip compile requirements.in -o requirements.txt

# Listar paquetes instalados
uv pip list

# Desinstalar paquetes
uv pip uninstall requests
```

#### Proyectos (pyproject.toml)

```bash
# Crear un nuevo proyecto
uv init my_project

# Instalar dependencias del proyecto
uv sync

# Agregar una dependencia al proyecto
uv add requests

# Agregar dependencia de desarrollo
uv add --dev pytest

# Remover una dependencia
uv remove requests

# Actualizar dependencias
uv sync --upgrade
```

#### Ejecución de Scripts

```bash
# Ejecutar un script Python dentro del entorno
uv run script.py

# Ejecutar un script con argumentos
uv run script.py --arg value

# Ejecutar un comando en el entorno
uv run python --version
```

### Configuración Común

#### pyproject.toml

Estructura básica para un proyecto con `uv`:

```toml
[project]
name = "my-project"
version = "0.1.0"
description = "Descripción del proyecto"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.28.0",
    "numpy",
]

[project.optional-dependencies]
dev = [
    "pytest",
    "black",
    "ruff",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

### Ventajas de uv

- **Velocidad**: Mucho más rápido que pip (hasta 100x en algunos casos)
- **Consistencia**: Bloqueo automático de versiones exactas
- **Simplicidad**: Una herramienta para múltiples funciones
- **Compatibilidad**: Funciona con pip y requirements.txt existentes

### Recursos

- [Documentación oficial](https://docs.astral.sh/uv/)
- [GitHub](https://github.com/astral-sh/uv)
