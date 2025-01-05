# CSS Básico - Guía de 1 Hora

Aprenderás los fundamentos de CSS para estilizar tus páginas web en solo 1 hora! 🚀

## Requisitos Previos
- Conocimientos básicos de HTML
- Un editor de código (VS Code recomendado)
- Un navegador web moderno

## 1. Introducción a CSS (5 minutos)

### Formas de incluir CSS
```html
<!-- 1. CSS Interno -->
<head>
    <style>
        p {
            color: blue;
        }
    </style>
</head>

<!-- 2. CSS en línea -->
<p style="color: blue;">Este es un párrafo azul</p>

<!-- 3. CSS Externo (recomendado) -->
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

## 2. Selectores Básicos (10 minutos)

```css
/* Selector de elemento */
p {
    color: blue;
}

/* Selector de clase */
.destacado {
    background-color: yellow;
}

/* Selector de ID */
#principal {
    font-size: 24px;
}

/* Selector descendente */
div p {
    margin-left: 20px;
}

/* Selector de hijo directo */
div > p {
    border: 1px solid black;
}

/* Múltiples selectores */
h1, h2, h3 {
    font-family: Arial;
}
```

## 3. Colores y Fondos (8 minutos)

```css
.ejemplo-colores {
    /* Colores */
    color: red;                       /* Nombre del color */
    color: #FF0000;                  /* Hexadecimal */
    color: rgb(255, 0, 0);           /* RGB */
    color: rgba(255, 0, 0, 0.5);     /* RGBA con transparencia */

    /* Fondos */
    background-color: #f0f0f0;
    background-image: url('fondo.jpg');
    background-repeat: no-repeat;
    background-position: center;
    background-size: cover;

    /* Forma corta */
    background: #f0f0f0 url('fondo.jpg') no-repeat center/cover;
}
```

## 4. Texto y Tipografía (8 minutos)

```css
.estilos-texto {
    /* Tipografía */
    font-family: Arial, sans-serif;
    font-size: 16px;
    font-weight: bold;
    font-style: italic;

    /* Texto */
    text-align: center;
    text-decoration: underline;
    text-transform: uppercase;
    line-height: 1.5;
    letter-spacing: 2px;
    word-spacing: 4px;
}
```

## 5. Modelo de Caja (10 minutos)

```css
.modelo-caja {
    /* Dimensiones */
    width: 300px;
    height: 200px;

    /* Padding (espacio interno) */
    padding-top: 10px;
    padding-right: 20px;
    padding-bottom: 10px;
    padding-left: 20px;
    /* Forma corta */
    padding: 10px 20px;

    /* Border */
    border-width: 1px;
    border-style: solid;
    border-color: black;
    /* Forma corta */
    border: 1px solid black;
    border-radius: 5px;

    /* Margin (espacio externo) */
    margin-top: 10px;
    margin-right: auto;
    margin-bottom: 10px;
    margin-left: auto;
    /* Forma corta */
    margin: 10px auto;
}
```

## 6. Display y Posicionamiento (10 minutos)

```css
/* Display */
.elemento-bloque {
    display: block;
    width: 100%;
}

.elemento-inline {
    display: inline;
}

.elemento-inline-block {
    display: inline-block;
    width: 150px;
}

/* Posicionamiento */
.posicion-relativa {
    position: relative;
    top: 10px;
    left: 20px;
}

.posicion-absoluta {
    position: absolute;
    top: 0;
    right: 0;
}

.posicion-fija {
    position: fixed;
    bottom: 20px;
    right: 20px;
}
```

## 7. Flexbox Básico (9 minutos)

```css
.contenedor-flex {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 10px;
}

.item-flex {
    flex: 1;
    min-width: 200px;
}
```

## Ejemplo Práctico Completo

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Básico - Ejemplo</title>
    <style>
        /* Reset básico */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* Estilos generales */
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            color: #333;
        }

        /* Header */
        .header {
            background-color: #2c3e50;
            color: white;
            padding: 1rem;
            text-align: center;
        }

        /* Navegación */
        .nav {
            display: flex;
            justify-content: center;
            background-color: #34495e;
            padding: 0.5rem;
        }

        .nav a {
            color: white;
            text-decoration: none;
            padding: 0.5rem 1rem;
            margin: 0 0.5rem;
        }

        .nav a:hover {
            background-color: #2c3e50;
            border-radius: 4px;
        }

        /* Contenido principal */
        .main {
            max-width: 800px;
            margin: 2rem auto;
            padding: 0 1rem;
        }

        /* Tarjetas */
        .card {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 1rem;
            margin-bottom: 1rem;
        }

        .card h2 {
            color: #2c3e50;
            margin-bottom: 0.5rem;
        }

        /* Footer */
        .footer {
            background-color: #2c3e50;
            color: white;
            text-align: center;
            padding: 1rem;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
    </style>
</head>
<body>
    <header class="header">
        <h1>Mi Sitio Web</h1>
    </header>

    <nav class="nav">
        <a href="#inicio">Inicio</a>
        <a href="#servicios">Servicios</a>
        <a href="#contacto">Contacto</a>
    </nav>

    <main class="main">
        <div class="card">
            <h2>Bienvenidos</h2>
            <p>Este es un ejemplo de CSS básico aplicado.</p>
        </div>
    </main>

    <footer class="footer">
        <p>© 2024 Mi Sitio Web</p>
    </footer>
</body>
</html>
```

## Ejercicios Prácticos

1. Crear un botón con hover efecto
2. Diseñar una tarjeta de producto
3. Hacer un menú de navegación responsive
4. Implementar un diseño de dos columnas con flexbox

## Recursos Adicionales
- [MDN CSS Reference](https://developer.mozilla.org/es/docs/Web/CSS)
- [CSS-Tricks](https://css-tricks.com/)
- [Flexbox Froggy](https://flexboxfroggy.com/) - Juego para aprender Flexbox
- [Can I Use](https://caniuse.com/) - Verificar soporte de propiedades CSS

¡Felicitaciones! 🎉 Has completado el tutorial básico de CSS. Ahora tienes los conocimientos fundamentales para estilizar tus páginas web.
