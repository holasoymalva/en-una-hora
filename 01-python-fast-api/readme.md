# FastAPI en 1 Hora - Guía Práctica

¡Aprenderás FastAPI desde cero hasta crear tu primera API REST funcional en solo 1 hora! 🚀

## Requisitos Previos
- Python 3.7+
- Conocimientos básicos de Python
- Editor de código (VS Code recomendado)

## Instalación (2 minutos)
```bash
# Crear entorno virtual
python -m venv env

# Activar entorno virtual
# Windows
.\env\Scripts\activate
# Linux/Mac
source env/bin/activate

# Instalar FastAPI y uvicorn
pip install fastapi uvicorn
```

## 1. Hola Mundo en FastAPI (5 minutos)

Crea un archivo `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "¡Hola Mundo!"}

# Ejecutar con: uvicorn main:app --reload
```

## 2. Rutas y Parámetros (10 minutos)

```python
from fastapi import FastAPI

app = FastAPI()

# Parámetros de ruta
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}

# Query parameters
@app.get("/productos/")
def get_productos(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

## 3. Modelos Pydantic (10 minutos)

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = None

app = FastAPI()

@app.post("/items/")
def create_item(item: Item):
    return item
```

## 4. CRUD Completo (20 minutos)

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

# Modelo de datos
class Todo(BaseModel):
    id: Optional[int] = None
    title: str
    description: str
    completed: bool = False

# Base de datos simulada
todos = []
counter = 1

# Crear TODO
@app.post("/todos/", response_model=Todo)
def create_todo(todo: Todo):
    global counter
    todo.id = counter
    counter += 1
    todos.append(todo)
    return todo

# Obtener todos los TODOs
@app.get("/todos/", response_model=List[Todo])
def get_todos():
    return todos

# Obtener un TODO específico
@app.get("/todos/{todo_id}", response_model=Todo)
def get_todo(todo_id: int):
    todo = next((todo for todo in todos if todo.id == todo_id), None)
    if todo is None:
        raise HTTPException(status_code=404, detail="Todo not found")
    return todo

# Actualizar TODO
@app.put("/todos/{todo_id}", response_model=Todo)
def update_todo(todo_id: int, updated_todo: Todo):
    todo_idx = next((idx for idx, todo in enumerate(todos) if todo.id == todo_id), None)
    if todo_idx is None:
        raise HTTPException(status_code=404, detail="Todo not found")
    
    updated_todo.id = todo_id
    todos[todo_idx] = updated_todo
    return updated_todo

# Eliminar TODO
@app.delete("/todos/{todo_id}")
def delete_todo(todo_id: int):
    todo_idx = next((idx for idx, todo in enumerate(todos) if todo.id == todo_id), None)
    if todo_idx is None:
        raise HTTPException(status_code=404, detail="Todo not found")
    
    todos.pop(todo_idx)
    return {"message": "Todo deleted successfully"}
```

## 5. Documentación Automática (5 minutos)

FastAPI genera automáticamente la documentación de tu API. Accede a:
- `/docs` para la documentación interactiva con Swagger UI
- `/redoc` para la documentación alternativa con ReDoc

## 6. Middleware y CORS (8 minutos)

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Configurar CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # Permite todos los orígenes en desarrollo
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## Prueba tu API 🧪

1. Ejecuta tu servidor:
```bash
uvicorn main:app --reload
```

2. Abre tu navegador y visita:
- http://localhost:8000/ - Endpoint raíz
- http://localhost:8000/docs - Documentación Swagger
- http://localhost:8000/redoc - Documentación ReDoc

## Estructura Recomendada para Proyectos Más Grandes

```
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── models/
│   │   └── __init__.py
│   ├── routers/
│   │   └── __init__.py
│   └── schemas/
│       └── __init__.py
├── requirements.txt
└── README.md
```

## Recursos Adicionales
- [Documentación oficial de FastAPI](https://fastapi.tiangolo.com/)
- [Tutorial Oficial](https://fastapi.tiangolo.com/tutorial/)
- [Awesome FastAPI](https://github.com/mjhea0/awesome-fastapi)

¡Felicitaciones! 🎉 Has completado el tutorial básico de FastAPI. Ahora tienes los conocimientos fundamentales para crear APIs REST con FastAPI.
