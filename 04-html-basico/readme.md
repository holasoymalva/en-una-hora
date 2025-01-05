# HTML Básico - Guía de 1 Hora

Aprenderás los fundamentos de HTML para crear tus primeras páginas web en solo 1 hora! 🚀

## Requisitos Previos
- Un editor de código (VS Code recomendado)
- Un navegador web moderno
- ¡Ganas de aprender!

## 1. Estructura Básica (5 minutos)
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Primera Página</title>
</head>
<body>
    <!-- Aquí va el contenido visible de tu página -->
    <h1>¡Hola Mundo!</h1>
</body>
</html>
```

## 2. Elementos de Texto (10 minutos)

### Encabezados
```html
<h1>Encabezado Principal</h1>
<h2>Encabezado Secundario</h2>
<h3>Encabezado Nivel 3</h3>
<h4>Encabezado Nivel 4</h4>
<h5>Encabezado Nivel 5</h5>
<h6>Encabezado Nivel 6</h6>
```

### Párrafos y Formato de Texto
```html
<p>Este es un párrafo normal.</p>
<p>Este párrafo contiene <strong>texto en negrita</strong> y <em>texto en cursiva</em>.</p>
<p>También puedes usar <b>negrita</b> y <i>cursiva</i> de esta forma.</p>
<p>Este es un texto con <u>subrayado</u> y este está <s>tachado</s>.</p>
```

## 3. Listas (8 minutos)

### Lista No Ordenada
```html
<ul>
    <li>Elemento 1</li>
    <li>Elemento 2</li>
    <li>Elemento 3</li>
</ul>
```

### Lista Ordenada
```html
<ol>
    <li>Primer paso</li>
    <li>Segundo paso</li>
    <li>Tercer paso</li>
</ol>
```

### Lista de Definición
```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
</dl>
```

## 4. Enlaces e Imágenes (10 minutos)

### Enlaces
```html
<!-- Enlace externo -->
<a href="https://www.google.com">Visita Google</a>

<!-- Enlace interno -->
<a href="contacto.html">Contacto</a>

<!-- Enlace que abre en nueva pestaña -->
<a href="https://www.github.com" target="_blank">GitHub</a>

<!-- Enlace a email -->
<a href="mailto:ejemplo@correo.com">Envíame un email</a>
```

### Imágenes
```html
<!-- Imagen básica -->
<img src="imagen.jpg" alt="Descripción de la imagen">

<!-- Imagen con dimensiones -->
<img src="logo.png" alt="Logo" width="200" height="100">

<!-- Imagen con enlace -->
<a href="https://www.ejemplo.com">
    <img src="banner.jpg" alt="Banner">
</a>
```

## 5. Tablas (10 minutos)
```html
<table border="1">
    <thead>
        <tr>
            <th>Encabezado 1</th>
            <th>Encabezado 2</th>
            <th>Encabezado 3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Fila 1, Celda 1</td>
            <td>Fila 1, Celda 2</td>
            <td>Fila 1, Celda 3</td>
        </tr>
        <tr>
            <td>Fila 2, Celda 1</td>
            <td>Fila 2, Celda 2</td>
            <td>Fila 2, Celda 3</td>
        </tr>
    </tbody>
</table>
```

## 6. Formularios (12 minutos)
```html
<form action="/enviar" method="POST">
    <!-- Campo de texto -->
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" name="nombre" required>
    <br><br>

    <!-- Email -->
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
    <br><br>

    <!-- Contraseña -->
    <label for="password">Contraseña:</label>
    <input type="password" id="password" name="password">
    <br><br>

    <!-- Área de texto -->
    <label for="mensaje">Mensaje:</label>
    <textarea id="mensaje" name="mensaje" rows="4" cols="50"></textarea>
    <br><br>

    <!-- Checkbox -->
    <input type="checkbox" id="subscribir" name="subscribir">
    <label for="subscribir">Subscribirse al boletín</label>
    <br><br>

    <!-- Radio buttons -->
    <input type="radio" id="html" name="lenguaje" value="html">
    <label for="html">HTML</label>
    <input type="radio" id="css" name="lenguaje" value="css">
    <label for="css">CSS</label>
    <br><br>

    <!-- Select -->
    <label for="pais">País:</label>
    <select id="pais" name="pais">
        <option value="">Selecciona un país</option>
        <option value="es">España</option>
        <option value="mx">México</option>
        <option value="ar">Argentina</option>
    </select>
    <br><br>

    <!-- Botón de envío -->
    <button type="submit">Enviar</button>
</form>
```

## 7. Elementos Semánticos (5 minutos)
```html
<header>
    <nav>
        <ul>
            <li><a href="#inicio">Inicio</a></li>
            <li><a href="#servicios">Servicios</a></li>
            <li><a href="#contacto">Contacto</a></li>
        </ul>
    </nav>
</header>

<main>
    <section id="inicio">
        <h1>Bienvenidos</h1>
        <p>Contenido principal...</p>
    </section>

    <article>
        <h2>Título del Artículo</h2>
        <p>Contenido del artículo...</p>
    </article>

    <aside>
        <h3>Contenido Relacionado</h3>
        <p>Enlaces y contenido adicional...</p>
    </aside>
</main>

<footer>
    <p>© 2024 Mi Sitio Web</p>
</footer>
```

## Ejercicio Práctico Final

Crea una página web simple que incluya:
1. Un encabezado con título y navegación
2. Una sección principal con texto e imágenes
3. Un formulario de contacto
4. Un pie de página

## Recursos Adicionales
- [MDN Web Docs - HTML](https://developer.mozilla.org/es/docs/Web/HTML)
- [W3Schools HTML Tutorial](https://www.w3schools.com/html/)
- [HTML Validator](https://validator.w3.org/)
- [Can I Use](https://caniuse.com/) - Para verificar compatibilidad de elementos

¡Felicitaciones! 🎉 Has completado el tutorial básico de HTML. Ahora tienes los conocimientos fundamentales para crear páginas web básicas.
