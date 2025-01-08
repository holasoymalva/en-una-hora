# Django - Guía de 1 Hora

Aprenderás los conceptos fundamentales de Django, uno de los frameworks web más populares de Python. 🚀

## Requisitos Previos
- Python 3.x instalado
- Conocimientos básicos de Python
- Familiaridad con HTML/CSS (opcional)
- Un editor de código (VS Code recomendado)

## 1. Configuración Inicial (5 minutos)

```bash
# Crear entorno virtual
python -m venv env

# Activar entorno virtual
# Windows
.\env\Scripts\activate
# Linux/Mac
source env/bin/activate

# Instalar Django
pip install django

# Crear proyecto
django-admin startproject mi_proyecto
cd mi_proyecto

# Crear aplicación
python manage.py startapp blog

# Ejecutar servidor de desarrollo
python manage.py runserver
```

## 2. Estructura del Proyecto (5 minutos)

```plaintext
mi_proyecto/
    ├── manage.py
    ├── mi_proyecto/
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── blog/
        ├── migrations/
        ├── __init__.py
        ├── admin.py
        ├── apps.py
        ├── models.py
        ├── views.py
        └── urls.py
```

> **Nota sobre la Estructura:**
> - `manage.py`: Utilidad de línea de comandos
> - `settings.py`: Configuración del proyecto
> - `urls.py`: Definición de URLs
> - `models.py`: Modelos de la base de datos
> - `views.py`: Lógica de la aplicación
> - `admin.py`: Configuración del admin

## 3. Modelos y Base de Datos (15 minutos)

```python
# blog/models.py
from django.db import models
from django.utils import timezone

class Post(models.Model):
    titulo = models.CharField(max_length=200)
    contenido = models.TextField()
    fecha_creacion = models.DateTimeField(default=timezone.now)
    autor = models.ForeignKey(
        'auth.User',
        on_delete=models.CASCADE
    )
    publicado = models.BooleanField(default=False)

    def __str__(self):
        return self.titulo

    class Meta:
        ordering = ['-fecha_creacion']

# Registrar en admin.py
from django.contrib import admin
from .models import Post

admin.site.register(Post)

# Crear y aplicar migraciones
# python manage.py makemigrations
# python manage.py migrate
```

> **Nota sobre Modelos:**
> - Los modelos definen la estructura de la base de datos
> - Cada clase es una tabla en la base de datos
> - Los campos definen las columnas
> - Las migraciones rastrean cambios en los modelos
> - El admin de Django proporciona una interfaz automática

## 4. Vistas y URLs (15 minutos)

```python
# blog/views.py
from django.shortcuts import render, get_object_or_404
from .models import Post

def lista_posts(request):
    posts = Post.objects.filter(publicado=True)
    return render(request, 'blog/lista_posts.html', {
        'posts': posts
    })

def detalle_post(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/detalle_post.html', {
        'post': post
    })

# blog/urls.py
from django.urls import path
from . import views

app_name = 'blog'

urlpatterns = [
    path('', views.lista_posts, name='lista_posts'),
    path('post/<int:pk>/', views.detalle_post, name='detalle_post'),
]

# mi_proyecto/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

> **Nota sobre Vistas y URLs:**
> - Las vistas manejan la lógica de la aplicación
> - Las URLs mapean URLs a vistas
> - `render()` combina un template con datos
> - `get_object_or_404()` maneja elegantemente los errores 404
> - El nombre de las URLs permite referencias inversas

## 5. Templates (10 minutos)

```html
<!-- blog/templates/blog/base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Mi Blog{% endblock %}</title>
</head>
<body>
    <header>
        <h1><a href="{% url 'blog:lista_posts' %}">Mi Blog</a></h1>
    </header>
    
    <main>
        {% block content %}
        {% endblock %}
    </main>
    
    <footer>
        <p>&copy; 2024 Mi Blog</p>
    </footer>
</body>
</html>

<!-- blog/templates/blog/lista_posts.html -->
{% extends 'blog/base.html' %}

{% block title %}Posts{% endblock %}

{% block content %}
    {% for post in posts %}
        <article>
            <h2>
                <a href="{% url 'blog:detalle_post' pk=post.pk %}">
                    {{ post.titulo }}
                </a>
            </h2>
            <p>{{ post.fecha_creacion|date:"d/m/Y" }}</p>
            <p>{{ post.contenido|truncatewords:30 }}</p>
        </article>
    {% empty %}
        <p>No hay posts publicados.</p>
    {% endfor %}
{% endblock %}

<!-- blog/templates/blog/detalle_post.html -->
{% extends 'blog/base.html' %}

{% block title %}{{ post.titulo }}{% endblock %}

{% block content %}
    <article>
        <h2>{{ post.titulo }}</h2>
        <p>
            Por {{ post.autor }} | 
            {{ post.fecha_creacion|date:"d/m/Y H:i" }}
        </p>
        <div>{{ post.contenido|linebreaks }}</div>
    </article>
{% endblock %}
```

> **Nota sobre Templates:**
> - Los templates definen la presentación HTML
> - `{% %}` para lógica de template
> - `{{ }}` para mostrar variables
> - `|` para filtros de template
> - La herencia de templates evita duplicación de código

## 6. Formularios (10 minutos)

```python
# blog/forms.py
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['titulo', 'contenido', 'publicado']

# blog/views.py
from django.shortcuts import redirect
from .forms import PostForm

def crear_post(request):
    if request.method == "POST":
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.autor = request.user
            post.save()
            return redirect('blog:detalle_post', pk=post.pk)
    else:
        form = PostForm()
    return render(request, 'blog/editar_post.html', {
        'form': form
    })
```

```html
<!-- blog/templates/blog/editar_post.html -->
{% extends 'blog/base.html' %}

{% block title %}Nuevo Post{% endblock %}

{% block content %}
    <h2>Nuevo Post</h2>
    <form method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Guardar</button>
    </form>
{% endblock %}
```

> **Nota sobre Formularios:**
> - Los formularios manejan la entrada de datos
> - `ModelForm` crea formularios basados en modelos
> - `csrf_token` protege contra ataques CSRF
> - `form.as_p` renderiza el formulario automáticamente
> - La validación ocurre automáticamente

## Recursos Adicionales
- [Documentación oficial de Django](https://docs.djangoproject.com/)
- [Tutorial de Django Girls](https://tutorial.djangogirls.org/)
- [Django Debug Toolbar](https://django-debug-toolbar.readthedocs.io/)
- [Awesome Django](https://github.com/wsvincent/awesome-django)

## Tips y Mejores Prácticas

1. **Seguridad**
   - Siempre usar `csrf_token` en formularios
   - No exponer información sensible en templates
   - Usar `get_object_or_404` para manejar 404s

2. **Organización**
   - Una app por funcionalidad principal
   - Mantener las vistas pequeñas y enfocadas
   - Usar mixins para código reutilizable

3. **Rendimiento**
   - Usar `select_related()` y `prefetch_related()`
   - Implementar caché cuando sea necesario
   - Optimizar consultas a la base de datos

4. **Despliegue**
   - Configurar `DEBUG = False` en producción
   - Usar variables de entorno para configuración
   - Servir archivos estáticos correctamente

¡Felicitaciones! 🎉 Has completado el tutorial de Django. Ahora tienes las bases necesarias para crear aplicaciones web con Django.
