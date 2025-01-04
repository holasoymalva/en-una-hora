# Fundamentos de Flask en 1 Hora - Guía Práctica

Aprenderás los fundamentos de Flask desde cero hasta crear tu primera aplicación web funcional en solo 1 hora! 🚀

## Requisitos Previos
- Python 3.6+
- Conocimientos básicos de Python
- Editor de código (VS Code recomendado)
- Nociones básicas de HTML

## Instalación (2 minutos)
```bash
# Crear entorno virtual
python -m venv env

# Activar entorno virtual
# Windows
.\env\Scripts\activate
# Linux/Mac
source env/bin/activate

# Instalar Flask
pip install flask
```

## 1. Hola Mundo en Flask (5 minutos)

Crea un archivo `app.py`:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<h1>¡Hola, Mundo!</h1>"

if __name__ == '__main__':
    app.run(debug=True)

# Ejecutar con: python app.py
```

## 2. Rutas y Métodos HTTP (10 minutos)

```python
from flask import Flask, request

app = Flask(__name__)

# Ruta básica
@app.route("/")
def home():
    return "Página principal"

# Ruta con parámetros
@app.route("/usuario/<nombre>")
def saludo(nombre):
    return f"¡Hola, {nombre}!"

# Ruta con métodos HTTP específicos
@app.route("/login", methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return "Procesando login..."
    return "Por favor, inicia sesión"

# Ruta con parámetros query
@app.route("/productos")
def productos():
    categoria = request.args.get('categoria', 'todos')
    return f"Mostrando productos de: {categoria}"
```

## 3. Templates con Jinja2 (15 minutos)

Estructura de carpetas:
```
├── app.py
└── templates/
    ├── base.html
    └── home.html
```

`templates/base.html`:
```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    <nav>
        <a href="{{ url_for('home') }}">Inicio</a>
        <a href="{{ url_for('about') }}">Acerca de</a>
    </nav>
    
    {% block content %}
    {% endblock %}
</body>
</html>
```

`templates/home.html`:
```html
{% extends "base.html" %}

{% block title %}Inicio{% endblock %}

{% block content %}
<h1>Bienvenido</h1>
<p>Esta es una página de ejemplo usando Flask y Jinja2</p>

{% if usuarios %}
    <h2>Usuarios:</h2>
    <ul>
    {% for usuario in usuarios %}
        <li>{{ usuario.nombre }}</li>
    {% endfor %}
    </ul>
{% endif %}
{% endblock %}
```

`app.py`:
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    usuarios = [
        {"nombre": "Ana"},
        {"nombre": "Juan"},
        {"nombre": "María"}
    ]
    return render_template('home.html', usuarios=usuarios)

@app.route("/about")
def about():
    return render_template('about.html')
```

## 4. Formularios y Datos (15 minutos)

`templates/formulario.html`:
```html
{% extends "base.html" %}

{% block content %}
<h1>Registro de Usuario</h1>
<form method="POST">
    <div>
        <label>Nombre:</label>
        <input type="text" name="nombre" required>
    </div>
    <div>
        <label>Email:</label>
        <input type="email" name="email" required>
    </div>
    <button type="submit">Registrar</button>
</form>

{% if mensaje %}
    <p>{{ mensaje }}</p>
{% endif %}
{% endblock %}
```

```python
from flask import Flask, render_template, request, redirect, url_for, flash

app = Flask(__name__)
app.secret_key = 'clave_secreta_aqui'  # Necesario para flash messages

usuarios = []

@app.route("/registro", methods=['GET', 'POST'])
def registro():
    if request.method == 'POST':
        nombre = request.form['nombre']
        email = request.form['email']
        
        usuarios.append({
            'nombre': nombre,
            'email': email
        })
        
        flash('Usuario registrado exitosamente!')
        return redirect(url_for('home'))
    
    return render_template('formulario.html')
```

## 5. Base de Datos Simple con SQLite (13 minutos)

```python
import sqlite3
from flask import g

DATABASE = 'database.db'

def get_db():
    db = getattr(g, '_database', None)
    if db is None:
        db = g._database = sqlite3.connect(DATABASE)
        db.row_factory = sqlite3.Row
    return db

@app.teardown_appcontext
def close_connection(exception):
    db = getattr(g, '_database', None)
    if db is not None:
        db.close()

# Inicializar la base de datos
def init_db():
    with app.app_context():
        db = get_db()
        with app.open_resource('schema.sql', mode='r') as f:
            db.cursor().executescript(f.read())
        db.commit()

# Ejemplo de consulta
@app.route("/usuarios")
def lista_usuarios():
    db = get_db()
    cur = db.execute('SELECT nombre, email FROM usuarios')
    usuarios = cur.fetchall()
    return render_template('usuarios.html', usuarios=usuarios)
```

`schema.sql`:
```sql
DROP TABLE IF EXISTS usuarios;
CREATE TABLE usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);
```

## Estructura Recomendada del Proyecto

```
├── app/
│   ├── __init__.py
│   ├── routes.py
│   ├── models.py
│   ├── templates/
│   │   ├── base.html
│   │   └── home.html
│   └── static/
│       ├── css/
│       └── js/
├── config.py
├── requirements.txt
└── run.py
```

## Recursos Adicionales
- [Documentación oficial de Flask](https://flask.palletsprojects.com/)
- [Tutorial Oficial](https://flask.palletsprojects.com/tutorial/)
- [Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)

¡Felicitaciones! 🎉 Has completado el tutorial de fundamentos de Flask. Ahora tienes las bases necesarias para crear aplicaciones web con Flask.
