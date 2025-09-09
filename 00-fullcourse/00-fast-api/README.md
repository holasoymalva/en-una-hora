# 🚀 FastAPI: De Cero a Desarrollador Mid-Level

> **Guía completa para dominar FastAPI y conseguir trabajo como desarrollador backend**

## 📋 Tabla de Contenidos

- [🎯 Objetivos del Curso](#-objetivos-del-curso)
- [📚 Prerrequisitos](#-prerrequisitos)
- [🏗️ Estructura del Proyecto](#️-estructura-del-proyecto)
- [📖 Módulo 1: Fundamentos](#-módulo-1-fundamentos)
- [📖 Módulo 2: APIs REST Avanzadas](#-módulo-2-apis-rest-avanzadas)
- [📖 Módulo 3: Base de Datos y ORM](#-módulo-3-base-de-datos-y-orm)
- [📖 Módulo 4: Autenticación y Seguridad](#-módulo-4-autenticación-y-seguridad)
- [📖 Módulo 5: Testing y Calidad](#-módulo-5-testing-y-calidad)
- [📖 Módulo 6: Deployment y DevOps](#-módulo-6-deployment-y-devops)
- [📖 Módulo 7: Proyecto Final](#-módulo-7-proyecto-final)
- [💼 Preparación para Entrevistas](#-preparación-para-entrevistas)
- [📚 Recursos Adicionales](#-recursos-adicionales)

---

## 🎯 Objetivos del Curso

Al completar este curso, serás capaz de:

- ✅ Construir APIs REST robustas y escalables con FastAPI
- ✅ Implementar autenticación y autorización segura
- ✅ Trabajar con bases de datos usando SQLAlchemy
- ✅ Escribir tests comprehensivos
- ✅ Desplegar aplicaciones en producción
- ✅ Aplicar mejores prácticas de desarrollo
- ✅ Prepararte para entrevistas técnicas

**Duración estimada:** 40-60 horas de estudio y práctica

---

## 📚 Prerrequisitos

- **Python 3.8+** con conocimientos intermedios
- **Conceptos básicos de APIs REST**
- **Conocimientos básicos de bases de datos**
- **Git y GitHub**
- **Terminal/Command Line**

---

## 🏗️ Estructura del Proyecto

```
fastapi-course/
├── 📁 modules/
│   ├── 📁 01-fundamentos/
│   ├── 📁 02-apis-avanzadas/
│   ├── 📁 03-database-orm/
│   ├── 📁 04-auth-security/
│   ├── 📁 05-testing/
│   ├── 📁 06-deployment/
│   └── 📁 07-proyecto-final/
├── 📁 projects/
│   ├── 📁 ecommerce-api/
│   ├── 📁 social-media-api/
│   └── 📁 real-time-chat/
├── 📁 exercises/
├── 📁 resources/
└── 📄 README.md
```

---

## 📖 Módulo 1: Fundamentos

### 🎯 Objetivos
- Entender qué es FastAPI y sus ventajas
- Configurar el entorno de desarrollo
- Crear tu primera API
- Comprender el sistema de tipos de Python

### 📝 Contenido

#### 1.1 Introducción a FastAPI
- ¿Qué es FastAPI?
- Ventajas sobre Flask y Django
- Casos de uso ideales
- Comparación con otros frameworks

#### 1.2 Configuración del Entorno
```bash
# Crear entorno virtual
python -m venv fastapi-env
source fastapi-env/bin/activate  # Linux/Mac
# fastapi-env\Scripts\activate   # Windows

# Instalar dependencias
pip install fastapi uvicorn[standard]
```

#### 1.3 Tu Primera API
```python
# main.py
from fastapi import FastAPI

app = FastAPI(title="Mi Primera API", version="1.0.0")

@app.get("/")
def read_root():
    return {"message": "¡Hola FastAPI!"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

#### 1.4 Modelos Pydantic
```python
from pydantic import BaseModel
from typing import Optional

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

class ItemResponse(Item):
    id: int
```

### 🏋️ Ejercicios Prácticos
1. Crear una API de gestión de productos
2. Implementar validación de datos
3. Agregar documentación personalizada

---

## 📖 Módulo 2: APIs REST Avanzadas

### 🎯 Objetivos
- Dominar todos los métodos HTTP
- Implementar CRUD completo
- Manejar errores y excepciones
- Optimizar rendimiento

### 📝 Contenido

#### 2.1 Operaciones CRUD Completas
```python
from fastapi import FastAPI, HTTPException, status
from typing import List

app = FastAPI()

# Base de datos en memoria (para ejemplo)
items_db = []
next_id = 1

@app.post("/items/", response_model=ItemResponse, status_code=status.HTTP_201_CREATED)
def create_item(item: Item):
    global next_id
    item_data = item.dict()
    item_data["id"] = next_id
    next_id += 1
    items_db.append(item_data)
    return item_data

@app.get("/items/", response_model=List[ItemResponse])
def read_items(skip: int = 0, limit: int = 10):
    return items_db[skip: skip + limit]

@app.get("/items/{item_id}", response_model=ItemResponse)
def read_item(item_id: int):
    for item in items_db:
        if item["id"] == item_id:
            return item
    raise HTTPException(status_code=404, detail="Item not found")

@app.put("/items/{item_id}", response_model=ItemResponse)
def update_item(item_id: int, item: Item):
    for i, existing_item in enumerate(items_db):
        if existing_item["id"] == item_id:
            updated_item = item.dict()
            updated_item["id"] = item_id
            items_db[i] = updated_item
            return updated_item
    raise HTTPException(status_code=404, detail="Item not found")

@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    for i, item in enumerate(items_db):
        if item["id"] == item_id:
            items_db.pop(i)
            return {"message": "Item deleted successfully"}
    raise HTTPException(status_code=404, detail="Item not found")
```

#### 2.2 Manejo de Errores
```python
from fastapi import HTTPException, Request
from fastapi.responses import JSONResponse

class CustomException(Exception):
    def __init__(self, message: str, status_code: int = 400):
        self.message = message
        self.status_code = status_code

@app.exception_handler(CustomException)
async def custom_exception_handler(request: Request, exc: CustomException):
    return JSONResponse(
        status_code=exc.status_code,
        content={"message": exc.message}
    )
```

#### 2.3 Middleware y CORS
```python
from fastapi.middleware.cors import CORSMiddleware
from fastapi.middleware.trustedhost import TrustedHostMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],  # Frontend URL
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

app.add_middleware(
    TrustedHostMiddleware, 
    allowed_hosts=["localhost", "*.example.com"]
)
```

### 🏋️ Ejercicios Prácticos
1. Crear API de blog con posts y comentarios
2. Implementar paginación avanzada
3. Agregar filtros y búsqueda

---

## 📖 Módulo 3: Base de Datos y ORM

### 🎯 Objetivos
- Configurar SQLAlchemy con FastAPI
- Implementar modelos de base de datos
- Manejar migraciones
- Optimizar consultas

### 📝 Contenido

#### 3.1 Configuración de SQLAlchemy
```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./app.db"
# Para PostgreSQL: "postgresql://user:password@localhost/dbname"

engine = create_engine(SQLALCHEMY_DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

#### 3.2 Modelos de Base de Datos
```python
# models.py
from sqlalchemy import Column, Integer, String, Float, Boolean, DateTime
from sqlalchemy.sql import func
from database import Base

class Item(Base):
    __tablename__ = "items"
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, index=True)
    description = Column(String)
    price = Column(Float)
    tax = Column(Float)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), onupdate=func.now())
```

#### 3.3 Schemas Pydantic
```python
# schemas.py
from pydantic import BaseModel
from datetime import datetime
from typing import Optional

class ItemBase(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

class ItemCreate(ItemBase):
    pass

class ItemUpdate(BaseModel):
    name: Optional[str] = None
    description: Optional[str] = None
    price: Optional[float] = None
    tax: Optional[float] = None
    is_active: Optional[bool] = None

class Item(ItemBase):
    id: int
    is_active: bool
    created_at: datetime
    updated_at: Optional[datetime] = None
    
    class Config:
        orm_mode = True
```

#### 3.4 Operaciones CRUD con Base de Datos
```python
# crud.py
from sqlalchemy.orm import Session
from models import Item
from schemas import ItemCreate, ItemUpdate

def get_item(db: Session, item_id: int):
    return db.query(Item).filter(Item.id == item_id).first()

def get_items(db: Session, skip: int = 0, limit: int = 100):
    return db.query(Item).offset(skip).limit(limit).all()

def create_item(db: Session, item: ItemCreate):
    db_item = Item(**item.dict())
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    return db_item

def update_item(db: Session, item_id: int, item: ItemUpdate):
    db_item = db.query(Item).filter(Item.id == item_id).first()
    if db_item:
        update_data = item.dict(exclude_unset=True)
        for field, value in update_data.items():
            setattr(db_item, field, value)
        db.commit()
        db.refresh(db_item)
    return db_item

def delete_item(db: Session, item_id: int):
    db_item = db.query(Item).filter(Item.id == item_id).first()
    if db_item:
        db.delete(db_item)
        db.commit()
    return db_item
```

### 🏋️ Ejercicios Prácticos
1. Crear sistema de usuarios con roles
2. Implementar relaciones entre modelos
3. Agregar índices para optimización

---

## 📖 Módulo 4: Autenticación y Seguridad

### 🎯 Objetivos
- Implementar JWT authentication
- Manejar roles y permisos
- Aplicar mejores prácticas de seguridad
- Proteger endpoints sensibles

### 📝 Contenido

#### 4.1 Configuración de JWT
```python
# auth.py
from datetime import datetime, timedelta
from typing import Optional
from jose import JWTError, jwt
from passlib.context import CryptContext
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

SECRET_KEY = "your-secret-key-here"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def verify_password(plain_password, hashed_password):
    return pwd_context.verify(plain_password, hashed_password)

def get_password_hash(password):
    return pwd_context.hash(password)

def create_access_token(data: dict, expires_delta: Optional[timedelta] = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt
```

#### 4.2 Modelo de Usuario
```python
# models.py
from sqlalchemy import Column, Integer, String, Boolean, DateTime
from sqlalchemy.sql import func

class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True, index=True)
    email = Column(String, unique=True, index=True)
    username = Column(String, unique=True, index=True)
    hashed_password = Column(String)
    is_active = Column(Boolean, default=True)
    is_admin = Column(Boolean, default=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
```

#### 4.3 Endpoints de Autenticación
```python
# main.py
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm

@app.post("/register")
def register(user: UserCreate, db: Session = Depends(get_db)):
    # Verificar si el usuario ya existe
    db_user = get_user_by_email(db, email=user.email)
    if db_user:
        raise HTTPException(
            status_code=400,
            detail="Email already registered"
        )
    
    # Crear nuevo usuario
    hashed_password = get_password_hash(user.password)
    db_user = User(
        email=user.email,
        username=user.username,
        hashed_password=hashed_password
    )
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return {"message": "User created successfully"}

@app.post("/token")
def login_for_access_token(form_data: OAuth2PasswordRequestForm = Depends(), db: Session = Depends(get_db)):
    user = authenticate_user(db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user.username}, expires_delta=access_token_expires
    )
    return {"access_token": access_token, "token_type": "bearer"}

@app.get("/users/me/", response_model=User)
def read_users_me(current_user: User = Depends(get_current_active_user)):
    return current_user
```

### 🏋️ Ejercicios Prácticos
1. Implementar sistema de roles (admin, user, moderator)
2. Crear middleware de autorización
3. Agregar rate limiting

---

## 📖 Módulo 5: Testing y Calidad

### 🎯 Objetivos
- Escribir tests unitarios y de integración
- Configurar CI/CD
- Implementar code coverage
- Aplicar linting y formatting

### 📝 Contenido

#### 5.1 Configuración de Testing
```python
# conftest.py
import pytest
from fastapi.testclient import TestClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from database import Base, get_db
from main import app

SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"
engine = create_engine(SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False})
TestingSessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

def override_get_db():
    try:
        db = TestingSessionLocal()
        yield db
    finally:
        db.close()

app.dependency_overrides[get_db] = override_get_db

@pytest.fixture
def client():
    Base.metadata.create_all(bind=engine)
    with TestClient(app) as c:
        yield c
    Base.metadata.drop_all(bind=engine)
```

#### 5.2 Tests Unitarios
```python
# test_main.py
import pytest
from fastapi.testclient import TestClient

def test_read_root(client: TestClient):
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "¡Hola FastAPI!"}

def test_create_item(client: TestClient):
    item_data = {
        "name": "Test Item",
        "description": "A test item",
        "price": 10.99,
        "tax": 1.50
    }
    response = client.post("/items/", json=item_data)
    assert response.status_code == 201
    data = response.json()
    assert data["name"] == item_data["name"]
    assert "id" in data

def test_get_item_not_found(client: TestClient):
    response = client.get("/items/999")
    assert response.status_code == 404
```

#### 5.3 Tests de Autenticación
```python
# test_auth.py
def test_register_user(client: TestClient):
    user_data = {
        "email": "test@example.com",
        "username": "testuser",
        "password": "testpassword"
    }
    response = client.post("/register", json=user_data)
    assert response.status_code == 200
    assert response.json()["message"] == "User created successfully"

def test_login_user(client: TestClient):
    # Primero registrar usuario
    user_data = {
        "email": "test@example.com",
        "username": "testuser",
        "password": "testpassword"
    }
    client.post("/register", json=user_data)
    
    # Luego hacer login
    login_data = {
        "username": "testuser",
        "password": "testpassword"
    }
    response = client.post("/token", data=login_data)
    assert response.status_code == 200
    data = response.json()
    assert "access_token" in data
    assert data["token_type"] == "bearer"
```

### 🏋️ Ejercicios Prácticos
1. Escribir tests para todos los endpoints
2. Configurar GitHub Actions para CI/CD
3. Implementar code coverage

---

## 📖 Módulo 6: Deployment y DevOps

### 🎯 Objetivos
- Desplegar en diferentes plataformas
- Configurar Docker
- Implementar monitoreo
- Aplicar mejores prácticas de producción

### 📝 Contenido

#### 6.1 Dockerización
```dockerfile
# Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/fastapi_db
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=fastapi_db
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

#### 6.2 Variables de Entorno
```python
# config.py
from pydantic import BaseSettings

class Settings(BaseSettings):
    app_name: str = "FastAPI App"
    database_url: str
    secret_key: str
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30
    
    class Config:
        env_file = ".env"

settings = Settings()
```

#### 6.3 Logging y Monitoreo
```python
# logging_config.py
import logging
import sys
from fastapi import Request
import time

# Configurar logging
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
    handlers=[
        logging.FileHandler("app.log"),
        logging.StreamHandler(sys.stdout)
    ]
)

logger = logging.getLogger(__name__)

# Middleware de logging
@app.middleware("http")
async def log_requests(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    
    logger.info(
        f"{request.method} {request.url} - "
        f"Status: {response.status_code} - "
        f"Time: {process_time:.4f}s"
    )
    
    return response
```

### 🏋️ Ejercicios Prácticos
1. Desplegar en Heroku/Railway
2. Configurar dominio personalizado
3. Implementar health checks

---

## 📖 Módulo 7: Proyecto Final

### 🎯 Objetivos
- Aplicar todos los conceptos aprendidos
- Crear un proyecto completo
- Preparar portfolio
- Documentar el proyecto

### 📝 Proyecto: E-commerce API

#### Características del Proyecto:
- ✅ Sistema de usuarios con roles
- ✅ Catálogo de productos
- ✅ Carrito de compras
- ✅ Sistema de órdenes
- ✅ Pagos (simulado)
- ✅ Notificaciones
- ✅ Dashboard de administración
- ✅ Tests comprehensivos
- ✅ Documentación completa

#### Estructura del Proyecto:
```
ecommerce-api/
├── 📁 app/
│   ├── 📁 api/
│   │   ├── 📁 v1/
│   │   │   ├── 📁 endpoints/
│   │   │   └── 📄 api.py
│   │   └── 📄 deps.py
│   ├── 📁 core/
│   │   ├── 📄 config.py
│   │   ├── 📄 security.py
│   │   └── 📄 logging.py
│   ├── 📁 crud/
│   ├── 📁 models/
│   ├── 📁 schemas/
│   ├── 📁 services/
│   └── 📄 main.py
├── 📁 tests/
├── 📁 docs/
├── 📄 requirements.txt
├── 📄 Dockerfile
└── 📄 README.md
```

### 🏋️ Entregables
1. Código fuente completo
2. Tests con >90% coverage
3. Documentación de API
4. README detallado
5. Demo en vivo

---

## 💼 Preparación para Entrevistas

### 🎯 Preguntas Técnicas Comunes

#### FastAPI Específicas:
1. **¿Cuáles son las ventajas de FastAPI sobre Flask?**
2. **¿Cómo funciona el sistema de tipos de FastAPI?**
3. **¿Qué es Pydantic y cómo se integra con FastAPI?**
4. **¿Cómo manejas la autenticación en FastAPI?**
5. **¿Cuál es la diferencia entre `Depends` y `BackgroundTasks`?**

#### Backend en General:
1. **¿Cómo diseñarías una API RESTful?**
2. **¿Qué es JWT y cuándo lo usarías?**
3. **¿Cómo optimizarías una API lenta?**
4. **¿Qué es CORS y por qué es importante?**
5. **¿Cómo manejarías errores en una API?**

### 🎯 Proyectos para Portfolio

1. **E-commerce API** - Sistema completo de tienda online
2. **Social Media API** - Red social con posts, likes, comentarios
3. **Real-time Chat API** - Chat en tiempo real con WebSockets
4. **Analytics API** - Sistema de métricas y reportes
5. **File Storage API** - Sistema de gestión de archivos

### 🎯 Habilidades Clave para el Trabajo

#### Técnicas:
- ✅ FastAPI y Python avanzado
- ✅ SQLAlchemy y bases de datos
- ✅ Testing (pytest, coverage)
- ✅ Docker y containerización
- ✅ Git y GitHub
- ✅ APIs REST y GraphQL
- ✅ Autenticación y seguridad
- ✅ Deployment y DevOps

#### Soft Skills:
- ✅ Documentación clara
- ✅ Code reviews
- ✅ Trabajo en equipo
- ✅ Resolución de problemas
- ✅ Comunicación técnica

---

## 📚 Recursos Adicionales

### 📖 Documentación Oficial
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://pydantic-docs.helpmanual.io/)
- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/)

### 🎥 Cursos y Tutoriales
- [FastAPI Tutorial - Official](https://fastapi.tiangolo.com/tutorial/)
- [Real Python FastAPI](https://realpython.com/fastapi-python-web-apis/)
- [FastAPI Course - FreeCodeCamp](https://www.youtube.com/watch?v=7t2alSnE2-I)

### 📚 Libros Recomendados
- "FastAPI Modern Python Web Development" - Bill Lubanovic
- "Python Web Development with FastAPI" - Francois Voron
- "Architecture Patterns with Python" - Harry Percival

### 🛠️ Herramientas Útiles
- [FastAPI Generator](https://github.com/tiangolo/full-stack-fastapi-postgresql)
- [FastAPI Users](https://github.com/fastapi-users/fastapi-users)
- [FastAPI Pagination](https://github.com/uriyyo/fastapi-pagination)

### 🌐 Comunidades
- [FastAPI Discord](https://discord.gg/VQjSZaeJmf)
- [Reddit r/FastAPI](https://www.reddit.com/r/FastAPI/)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/fastapi)

---

## 🎉 ¡Felicitaciones!

Has completado la guía completa de FastAPI. Ahora tienes todas las herramientas necesarias para:

- 🚀 Construir APIs robustas y escalables
- 💼 Aplicar a trabajos de desarrollador backend
- 🏆 Crear proyectos impresionantes para tu portfolio
- 📈 Continuar tu crecimiento profesional

### 📝 Próximos Pasos

1. **Completa todos los ejercicios** de cada módulo
2. **Construye el proyecto final** paso a paso
3. **Crea tu portfolio** con 2-3 proyectos
4. **Practica entrevistas** técnicas
5. **Aplica a trabajos** y ¡consigue ese empleo!

---

**¡Recuerda: La práctica hace al maestro! 🎯**

*Mantén este repositorio actualizado y compártelo con la comunidad. ¡El conocimiento se multiplica cuando se comparte!*
