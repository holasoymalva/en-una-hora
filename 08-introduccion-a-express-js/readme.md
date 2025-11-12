# Introducción a Express.js

## ¿Qué es Express.js?

Express.js es un framework web minimalista y flexible para Node.js que proporciona un conjunto robusto de características para aplicaciones web y móviles. Es el framework más popular del ecosistema Node.js y facilita la creación de APIs y aplicaciones web de manera rápida y sencilla.

### Características principales:
- **Minimalista**: No impone una estructura rígida, te da libertad para organizar tu código
- **Middleware**: Sistema de funciones intermedias que procesan las peticiones
- **Routing**: Sistema de rutas simple y potente
- **Rendimiento**: Rápido y eficiente
- **Gran comunidad**: Miles de paquetes y middleware disponibles

---

## Requisitos previos

Antes de comenzar, asegúrate de tener instalado:
- **Node.js** (versión 14 o superior)
- **npm** (viene incluido con Node.js)
- Un editor de código (VS Code recomendado)
- Conocimientos básicos de JavaScript y Node.js

Para verificar tu instalación:
```bash
node --version
npm --version
```

---

## Tabla de contenidos

1. [Configuración inicial del proyecto](#1-configuración-inicial-del-proyecto)
2. [Tu primera aplicación Express](#2-tu-primera-aplicación-express)
3. [Entendiendo el routing](#3-entendiendo-el-routing)
4. [Middleware en Express](#4-middleware-en-express)
5. [Manejo de parámetros y query strings](#5-manejo-de-parámetros-y-query-strings)
6. [Sirviendo archivos estáticos](#6-sirviendo-archivos-estáticos)
7. [Trabajando con JSON](#7-trabajando-con-json)
8. [Manejo de errores](#8-manejo-de-errores)
9. [Proyecto práctico: API REST básica](#9-proyecto-práctico-api-rest-básica)
10. [Buenas prácticas](#10-buenas-prácticas)

---

## 1. Configuración inicial del proyecto

### Paso 1: Crear el directorio del proyecto

```bash
mkdir mi-app-express
cd mi-app-express
```

### Paso 2: Inicializar npm

```bash
npm init -y
```

Este comando crea un archivo `package.json` con la configuración por defecto.

### Paso 3: Instalar Express

```bash
npm install express
```

### Paso 4: Instalar nodemon (opcional pero recomendado)

Nodemon reinicia automáticamente el servidor cuando detecta cambios en los archivos:

```bash
npm install --save-dev nodemon
```

### Paso 5: Configurar scripts en package.json

Abre `package.json` y modifica la sección de scripts:

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  }
}
```

---

## 2. Tu primera aplicación Express

### Crear el archivo principal

Crea un archivo llamado `index.js` en la raíz del proyecto:

```javascript
// Importar Express
const express = require('express');

// Crear una instancia de Express
const app = express();

// Definir el puerto
const PORT = 3000;

// Ruta básica
app.get('/', (req, res) => {
  res.send('¡Hola Mundo con Express!');
});

// Iniciar el servidor
app.listen(PORT, () => {
  console.log(`Servidor corriendo en http://localhost:${PORT}`);
});
```

### Ejecutar la aplicación

```bash
npm run dev
```

Abre tu navegador y visita `http://localhost:3000`. Deberías ver "¡Hola Mundo con Express!".

### Explicación del código:

1. **`require('express')`**: Importa el módulo Express
2. **`express()`**: Crea una aplicación Express
3. **`app.get()`**: Define una ruta que responde a peticiones GET
4. **`req`**: Objeto de petición (request) con información del cliente
5. **`res`**: Objeto de respuesta (response) para enviar datos al cliente
6. **`app.listen()`**: Inicia el servidor en el puerto especificado

---

## 3. Entendiendo el routing

El routing determina cómo una aplicación responde a las peticiones del cliente en diferentes endpoints (URLs).

### Métodos HTTP básicos

```javascript
// GET - Obtener datos
app.get('/usuarios', (req, res) => {
  res.send('Lista de usuarios');
});

// POST - Crear datos
app.post('/usuarios', (req, res) => {
  res.send('Usuario creado');
});

// PUT - Actualizar datos completos
app.put('/usuarios/:id', (req, res) => {
  res.send(`Usuario ${req.params.id} actualizado`);
});

// PATCH - Actualizar datos parciales
app.patch('/usuarios/:id', (req, res) => {
  res.send(`Usuario ${req.params.id} actualizado parcialmente`);
});

// DELETE - Eliminar datos
app.delete('/usuarios/:id', (req, res) => {
  res.send(`Usuario ${req.params.id} eliminado`);
});
```

### Rutas con múltiples handlers

```javascript
app.get('/perfil', 
  (req, res, next) => {
    console.log('Verificando autenticación...');
    next(); // Pasa al siguiente handler
  },
  (req, res) => {
    res.send('Perfil del usuario');
  }
);
```

### Organizar rutas en módulos

Crea un archivo `routes/usuarios.js`:

```javascript
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.send('Lista de usuarios');
});

router.get('/:id', (req, res) => {
  res.send(`Usuario con ID: ${req.params.id}`);
});

router.post('/', (req, res) => {
  res.send('Usuario creado');
});

module.exports = router;
```

En tu `index.js`:

```javascript
const usuariosRoutes = require('./routes/usuarios');
app.use('/usuarios', usuariosRoutes);
```

---

## 4. Middleware en Express

Los middleware son funciones que tienen acceso a los objetos de petición (`req`), respuesta (`res`) y la siguiente función middleware (`next`).

### Middleware de aplicación

```javascript
// Middleware que se ejecuta en todas las peticiones
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next(); // Importante: llamar next() para continuar
});
```

### Middleware incorporado

```javascript
// Para parsear JSON en el body de las peticiones
app.use(express.json());

// Para parsear datos de formularios
app.use(express.urlencoded({ extended: true }));

// Para servir archivos estáticos
app.use(express.static('public'));
```

### Middleware de terceros

Instalar y usar `morgan` para logging:

```bash
npm install morgan
```

```javascript
const morgan = require('morgan');
app.use(morgan('dev'));
```

### Middleware personalizado

```javascript
// Middleware de autenticación simple
const autenticar = (req, res, next) => {
  const token = req.headers['authorization'];
  
  if (token === 'mi-token-secreto') {
    next(); // Usuario autenticado, continuar
  } else {
    res.status(401).json({ error: 'No autorizado' });
  }
};

// Usar el middleware en rutas específicas
app.get('/admin', autenticar, (req, res) => {
  res.send('Panel de administración');
});
```

---

## 5. Manejo de parámetros y query strings

### Parámetros de ruta (Route params)

```javascript
// URL: /usuarios/123
app.get('/usuarios/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`Usuario ID: ${userId}`);
});

// Múltiples parámetros
// URL: /posts/2023/12/mi-articulo
app.get('/posts/:year/:month/:slug', (req, res) => {
  const { year, month, slug } = req.params;
  res.json({ year, month, slug });
});
```

### Query strings

```javascript
// URL: /buscar?q=express&limit=10
app.get('/buscar', (req, res) => {
  const query = req.query.q;
  const limit = req.query.limit || 20; // Valor por defecto
  
  res.json({
    busqueda: query,
    limite: limit
  });
});
```

### Body de la petición

```javascript
// Asegúrate de tener app.use(express.json())

app.post('/usuarios', (req, res) => {
  const { nombre, email } = req.body;
  
  res.json({
    mensaje: 'Usuario creado',
    usuario: { nombre, email }
  });
});
```

---

## 6. Sirviendo archivos estáticos

### Configuración básica

```javascript
// Servir archivos desde la carpeta 'public'
app.use(express.static('public'));
```

Estructura de carpetas:
```
mi-app-express/
├── public/
│   ├── index.html
│   ├── styles.css
│   └── script.js
├── index.js
└── package.json
```

Ahora puedes acceder a:
- `http://localhost:3000/index.html`
- `http://localhost:3000/styles.css`
- `http://localhost:3000/script.js`

### Múltiples directorios estáticos

```javascript
app.use(express.static('public'));
app.use(express.static('files'));
app.use('/assets', express.static('assets'));
```

---

## 7. Trabajando con JSON

Express facilita el trabajo con JSON, el formato estándar para APIs.

### Enviar respuestas JSON

```javascript
app.get('/api/productos', (req, res) => {
  const productos = [
    { id: 1, nombre: 'Laptop', precio: 999 },
    { id: 2, nombre: 'Mouse', precio: 25 },
    { id: 3, nombre: 'Teclado', precio: 75 }
  ];
  
  res.json(productos);
});
```

### Recibir datos JSON

```javascript
// Middleware necesario
app.use(express.json());

app.post('/api/productos', (req, res) => {
  const nuevoProducto = req.body;
  
  // Validación básica
  if (!nuevoProducto.nombre || !nuevoProducto.precio) {
    return res.status(400).json({ 
      error: 'Nombre y precio son requeridos' 
    });
  }
  
  // Simular guardado
  nuevoProducto.id = Date.now();
  
  res.status(201).json({
    mensaje: 'Producto creado',
    producto: nuevoProducto
  });
});
```

### Códigos de estado HTTP comunes

```javascript
res.status(200).json({ mensaje: 'OK' });           // Éxito
res.status(201).json({ mensaje: 'Creado' });       // Recurso creado
res.status(400).json({ error: 'Bad Request' });    // Error del cliente
res.status(401).json({ error: 'No autorizado' });  // Sin autenticación
res.status(404).json({ error: 'No encontrado' }); // Recurso no existe
res.status(500).json({ error: 'Error servidor' }); // Error del servidor
```

---

## 8. Manejo de errores

### Middleware de manejo de errores

```javascript
// Debe ir al final, después de todas las rutas
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({
    error: 'Algo salió mal',
    mensaje: err.message
  });
});
```

### Ruta 404 (No encontrado)

```javascript
// Debe ir después de todas las rutas pero antes del error handler
app.use((req, res) => {
  res.status(404).json({
    error: 'Ruta no encontrada'
  });
});
```

### Manejo de errores asíncronos

```javascript
// Función helper para capturar errores async
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Uso
app.get('/datos', asyncHandler(async (req, res) => {
  const datos = await obtenerDatosAsync();
  res.json(datos);
}));
```

---

## 9. Proyecto práctico: API REST básica

Vamos a crear una API completa para gestionar una lista de tareas (TODO list).

### Estructura del proyecto

```
mi-app-express/
├── controllers/
│   └── tareasController.js
├── routes/
│   └── tareas.js
├── data/
│   └── tareas.js
├── index.js
└── package.json
```

### data/tareas.js (Base de datos simulada)

```javascript
let tareas = [
  { id: 1, titulo: 'Aprender Express', completada: false },
  { id: 2, titulo: 'Crear una API', completada: false }
];

let nextId = 3;

module.exports = {
  obtenerTodas: () => tareas,
  
  obtenerPorId: (id) => tareas.find(t => t.id === parseInt(id)),
  
  crear: (tarea) => {
    const nuevaTarea = {
      id: nextId++,
      titulo: tarea.titulo,
      completada: false
    };
    tareas.push(nuevaTarea);
    return nuevaTarea;
  },
  
  actualizar: (id, datosActualizados) => {
    const index = tareas.findIndex(t => t.id === parseInt(id));
    if (index === -1) return null;
    
    tareas[index] = { ...tareas[index], ...datosActualizados };
    return tareas[index];
  },
  
  eliminar: (id) => {
    const index = tareas.findIndex(t => t.id === parseInt(id));
    if (index === -1) return false;
    
    tareas.splice(index, 1);
    return true;
  }
};
```


### controllers/tareasController.js

```javascript
const tareasDB = require('../data/tareas');

// Obtener todas las tareas
exports.obtenerTodas = (req, res) => {
  const tareas = tareasDB.obtenerTodas();
  res.json(tareas);
};

// Obtener una tarea por ID
exports.obtenerPorId = (req, res) => {
  const tarea = tareasDB.obtenerPorId(req.params.id);
  
  if (!tarea) {
    return res.status(404).json({ error: 'Tarea no encontrada' });
  }
  
  res.json(tarea);
};

// Crear una nueva tarea
exports.crear = (req, res) => {
  const { titulo } = req.body;
  
  if (!titulo) {
    return res.status(400).json({ error: 'El título es requerido' });
  }
  
  const nuevaTarea = tareasDB.crear({ titulo });
  res.status(201).json(nuevaTarea);
};

// Actualizar una tarea
exports.actualizar = (req, res) => {
  const tareaActualizada = tareasDB.actualizar(req.params.id, req.body);
  
  if (!tareaActualizada) {
    return res.status(404).json({ error: 'Tarea no encontrada' });
  }
  
  res.json(tareaActualizada);
};

// Eliminar una tarea
exports.eliminar = (req, res) => {
  const eliminada = tareasDB.eliminar(req.params.id);
  
  if (!eliminada) {
    return res.status(404).json({ error: 'Tarea no encontrada' });
  }
  
  res.json({ mensaje: 'Tarea eliminada correctamente' });
};
```

### routes/tareas.js

```javascript
const express = require('express');
const router = express.Router();
const tareasController = require('../controllers/tareasController');

// Rutas
router.get('/', tareasController.obtenerTodas);
router.get('/:id', tareasController.obtenerPorId);
router.post('/', tareasController.crear);
router.put('/:id', tareasController.actualizar);
router.delete('/:id', tareasController.eliminar);

module.exports = router;
```

### index.js (Aplicación completa)

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Logger personalizado
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
});

// Rutas
const tareasRoutes = require('./routes/tareas');
app.use('/api/tareas', tareasRoutes);

// Ruta raíz
app.get('/', (req, res) => {
  res.json({
    mensaje: 'API de Tareas',
    endpoints: {
      'GET /api/tareas': 'Obtener todas las tareas',
      'GET /api/tareas/:id': 'Obtener una tarea por ID',
      'POST /api/tareas': 'Crear una nueva tarea',
      'PUT /api/tareas/:id': 'Actualizar una tarea',
      'DELETE /api/tareas/:id': 'Eliminar una tarea'
    }
  });
});

// Manejo de rutas no encontradas
app.use((req, res) => {
  res.status(404).json({ error: 'Ruta no encontrada' });
});

// Manejo de errores
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Error interno del servidor' });
});

// Iniciar servidor
app.listen(PORT, () => {
  console.log(`🚀 Servidor corriendo en http://localhost:${PORT}`);
});
```

### Probar la API

Puedes usar herramientas como:
- **Postman**
- **Insomnia**
- **Thunder Client** (extensión de VS Code)
- **curl** desde la terminal

#### Ejemplos con curl:

```bash
# Obtener todas las tareas
curl http://localhost:3000/api/tareas

# Obtener una tarea específica
curl http://localhost:3000/api/tareas/1

# Crear una nueva tarea
curl -X POST http://localhost:3000/api/tareas \
  -H "Content-Type: application/json" \
  -d '{"titulo": "Nueva tarea"}'

# Actualizar una tarea
curl -X PUT http://localhost:3000/api/tareas/1 \
  -H "Content-Type: application/json" \
  -d '{"completada": true}'

# Eliminar una tarea
curl -X DELETE http://localhost:3000/api/tareas/1
```

---

## 10. Buenas prácticas

### 1. Estructura de proyecto organizada

```
mi-app-express/
├── config/           # Configuraciones
├── controllers/      # Lógica de negocio
├── routes/          # Definición de rutas
├── middleware/      # Middleware personalizado
├── models/          # Modelos de datos
├── utils/           # Funciones auxiliares
├── public/          # Archivos estáticos
├── .env             # Variables de entorno
├── .gitignore       # Archivos a ignorar en git
├── index.js         # Punto de entrada
└── package.json
```

### 2. Variables de entorno

Instalar dotenv:
```bash
npm install dotenv
```

Crear archivo `.env`:
```
PORT=3000
NODE_ENV=development
API_KEY=tu-api-key-secreta
```

Usar en tu aplicación:
```javascript
require('dotenv').config();

const PORT = process.env.PORT || 3000;
const API_KEY = process.env.API_KEY;
```

### 3. Validación de datos

Instalar express-validator:
```bash
npm install express-validator
```

Ejemplo de uso:
```javascript
const { body, validationResult } = require('express-validator');

app.post('/usuarios',
  // Validaciones
  body('email').isEmail().withMessage('Email inválido'),
  body('password').isLength({ min: 6 }).withMessage('Mínimo 6 caracteres'),
  
  // Handler
  (req, res) => {
    const errors = validationResult(req);
    
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    
    // Procesar datos válidos
    res.json({ mensaje: 'Usuario creado' });
  }
);
```

### 4. Seguridad básica con Helmet

```bash
npm install helmet
```

```javascript
const helmet = require('helmet');
app.use(helmet());
```

### 5. CORS (Cross-Origin Resource Sharing)

```bash
npm install cors
```

```javascript
const cors = require('cors');

// Permitir todos los orígenes (desarrollo)
app.use(cors());

// O configurar específicamente
app.use(cors({
  origin: 'http://localhost:3001',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  credentials: true
}));
```

### 6. Compresión de respuestas

```bash
npm install compression
```

```javascript
const compression = require('compression');
app.use(compression());
```

### 7. Rate limiting (Limitar peticiones)

```bash
npm install express-rate-limit
```

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100, // Máximo 100 peticiones por ventana
  message: 'Demasiadas peticiones, intenta más tarde'
});

app.use('/api/', limiter);
```

### 8. Logging profesional

```bash
npm install winston
```

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}
```

### 9. Separar configuración del servidor

**app.js**:
```javascript
const express = require('express');
const app = express();

// Middleware
app.use(express.json());

// Rutas
app.get('/', (req, res) => {
  res.send('Hola Mundo');
});

module.exports = app;
```

**server.js**:
```javascript
const app = require('./app');
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Servidor en puerto ${PORT}`);
});
```

Esto facilita las pruebas y la separación de responsabilidades.

### 10. Manejo de promesas y async/await

```javascript
// Evitar try-catch repetitivo con un wrapper
const asyncHandler = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Uso
app.get('/datos', asyncHandler(async (req, res) => {
  const datos = await obtenerDatos();
  res.json(datos);
}));
```

---

## Recursos adicionales

### Documentación oficial
- [Express.js Official Docs](https://expressjs.com/)
- [Node.js Documentation](https://nodejs.org/docs/)

### Paquetes útiles
- **nodemon**: Reinicio automático del servidor
- **dotenv**: Variables de entorno
- **morgan**: HTTP request logger
- **helmet**: Seguridad HTTP headers
- **cors**: Cross-Origin Resource Sharing
- **express-validator**: Validación de datos
- **compression**: Compresión gzip
- **express-rate-limit**: Rate limiting

### Herramientas de testing
- **Postman**: Cliente API con interfaz gráfica
- **Insomnia**: Alternativa a Postman
- **Thunder Client**: Extensión de VS Code
- **curl**: Cliente de línea de comandos

---

## Ejercicios prácticos

### Ejercicio 1: Blog API
Crea una API REST para un blog con:
- Posts (título, contenido, autor, fecha)
- Comentarios (contenido, autor, postId)
- CRUD completo para ambos recursos

### Ejercicio 2: Sistema de autenticación
Implementa:
- Registro de usuarios
- Login con generación de token
- Middleware de autenticación
- Rutas protegidas

### Ejercicio 3: API de productos
Crea una API de e-commerce con:
- Productos (nombre, precio, stock, categoría)
- Categorías
- Filtrado y búsqueda
- Paginación

### Ejercicio 4: Upload de archivos
Implementa:
- Subida de imágenes
- Validación de tipo y tamaño
- Almacenamiento en servidor
- Servir las imágenes

---

## Troubleshooting (Solución de problemas)

### Error: Cannot GET /ruta
- Verifica que la ruta esté definida correctamente
- Asegúrate de usar el método HTTP correcto (GET, POST, etc.)
- Revisa que el servidor esté corriendo

### Error: Cannot find module 'express'
- Ejecuta `npm install` para instalar las dependencias
- Verifica que `express` esté en `package.json`

### Puerto ya en uso
```bash
# En Linux/Mac
lsof -ti:3000 | xargs kill -9

# En Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### req.body es undefined
- Asegúrate de usar `app.use(express.json())`
- Verifica que el Content-Type sea `application/json`
- Coloca el middleware antes de las rutas

### CORS errors
- Instala y configura el paquete `cors`
- Verifica los headers de la petición
- Asegúrate de permitir el origen correcto

---

## Conclusión

Express.js es una herramienta fundamental en el ecosistema de Node.js que te permite crear aplicaciones web y APIs de manera rápida y eficiente. Con esta guía has aprendido:

✅ Configurar un proyecto Express desde cero  
✅ Crear rutas y manejar diferentes métodos HTTP  
✅ Trabajar con middleware  
✅ Manejar parámetros, query strings y body  
✅ Servir archivos estáticos  
✅ Crear una API REST completa  
✅ Implementar buenas prácticas y seguridad  

### Próximos pasos

1. **Conectar con bases de datos**: MongoDB (Mongoose), PostgreSQL (Sequelize)
2. **Autenticación avanzada**: JWT, OAuth, Passport.js
3. **Testing**: Jest, Mocha, Supertest
4. **Despliegue**: Heroku, Vercel, AWS, DigitalOcean
5. **WebSockets**: Socket.io para comunicación en tiempo real
6. **GraphQL**: Apollo Server con Express

---

## Contacto y contribuciones

Si encuentras errores o tienes sugerencias para mejorar esta guía, no dudes en contribuir.

**¡Feliz coding! 🚀**

---

*Última actualización: Noviembre 2025*
