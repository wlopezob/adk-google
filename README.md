# Proyecto Python con uv

Este proyecto utiliza [uv](https://github.com/astral-sh/uv) para la gestión del entorno virtual y dependencias en Python.

## Requisitos previos
- Python 3.8 o superior
- uv instalado globalmente (`pip install uv` o consulta la documentación oficial)

## Creación del entorno virtual

1. Crea el entorno virtual con uv:
   ```sh
   uv venv .venv
   ```
2. Activa el entorno virtual:
   ```sh
   source .venv/bin/activate
   ```

## Instalación de dependencias

Para instalar dependencias, usa:
```sh
uv pip install <paquete>
```
Esto instalará el paquete dentro del entorno virtual usando uv.

## Añadir dependencias al proyecto

Puedes crear un archivo `requirements.txt` y luego instalar todo con:
```sh
uv pip install -r requirements.txt
```

## Notas
- uv es compatible con la mayoría de los flujos de trabajo de pip.
- Para más información, consulta la [documentación oficial de uv](https://github.com/astral-sh/uv).

---

### Estructura recomendada
- `.venv/` — Entorno virtual
- `requirements.txt` — Dependencias del proyecto (opcional)
- `src/` — Código fuente (opcional)

---

### Crear proyecto
```sh
uv init
```

## ADK
https://google.github.io/adk-docs

## Model google
https://google.github.io/adk-docs/agents/models/
