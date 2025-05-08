# Introducción a Node.js

## ¿Qué es Node.js?

Node.js es un entorno de ejecución de JavaScript del lado del servidor, basado en el motor V8 de Chrome. Permite ejecutar código JavaScript fuera del navegador, lo que posibilita su uso para crear aplicaciones de servidor, herramientas de línea de comandos y más.

> **Nota**: A diferencia de los entornos tradicionales como PHP o Java, Node.js utiliza un modelo de programación asíncrono y orientado a eventos, lo que lo hace muy eficiente para operaciones de entrada/salida.

## Instalación de Node.js

### Windows, macOS y Linux
1. Visita [nodejs.org](https://nodejs.org)
2. Descarga la versión LTS (Long Term Support) recomendada para la mayoría de usuarios
3. Sigue el asistente de instalación

### Verificación de la instalación
Para verificar que Node.js se instaló correctamente, abre una terminal y ejecuta:

```bash
node -v
npm -v
```

Estos comandos mostrarán las versiones de Node.js y npm (Node Package Manager) instaladas.

> **Nota**: npm es el gestor de paquetes que viene incluido con Node.js y te permite instalar, compartir y gestionar dependencias en tus proyectos.

## Primer programa en Node.js

Vamos a crear un simple "Hola Mundo" para entender cómo funciona Node.js.

1. Crea un archivo llamado `hola.js` con el siguiente contenido:

```javascript
console.log("¡Hola, mundo desde Node.js!");
```

2. Abre una terminal, navega hasta la ubicación del archivo y ejecútalo:

```bash
node hola.js
```

Verás el mensaje "¡Hola, mundo desde Node.js!" en la consola.

> **Nota**: A diferencia del JavaScript en el navegador, en Node.js `console.log()` imprime en la terminal en lugar de en la consola del navegador.

## Módulos en Node.js

Un concepto fundamental en Node.js es el sistema de módulos, que permite organizar el código en archivos separados y reutilizables.

### Módulos integrados (Core modules)

Node.js incluye varios módulos integrados que proporcionan funcionalidades esenciales:

```javascript
// Ejemplo usando el módulo 'path' para manipular rutas de archivos
const path = require('path');

const archivo = '/usuarios/documentos/archivo.txt';
console.log(path.basename(archivo)); // Imprime: archivo.txt
console.log(path.dirname(archivo));  // Imprime: /usuarios/documentos
console.log(path.extname(archivo));  // Imprime: .txt
```

> **Nota**: `require()` es la función que se utiliza para importar módulos en Node.js. En versiones más recientes también se soporta la sintaxis de importación de ES modules (`import/export`).

### Creando tus propios módulos

Puedes crear tus propios módulos para organizar mejor tu código:

1. Crea un archivo llamado `matematicas.js`:

```javascript
// matematicas.js
function sumar(a, b) {
  return a + b;
}

function restar(a, b) {
  return a - b;
}

// Exportamos las funciones para que estén disponibles al importar el módulo
module.exports = {
  sumar,
  restar
};
```

2. Crea otro archivo llamado `app.js` que use este módulo:

```javascript
// app.js
const matematicas = require('./matematicas');

console.log(matematicas.sumar(5, 3)); // Imprime: 8
console.log(matematicas.restar(10, 4)); // Imprime: 6
```

> **Nota**: `module.exports` es un objeto especial en Node.js que determina qué será accesible cuando importes el módulo con `require()`.

## Servidor HTTP básico

Uno de los usos más comunes de Node.js es crear servidores web. Vamos a crear un servidor HTTP simple:

```javascript
// servidor.js
const http = require('http');

// Crear un servidor que responda con "¡Hola desde mi servidor Node.js!"
const servidor = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('¡Hola desde mi servidor Node.js!\n');
});

// Configurar el puerto y la dirección IP
const puerto = 3000;
const host = 'localhost';

// Iniciar el servidor
servidor.listen(puerto, host, () => {
  console.log(`Servidor ejecutándose en http://${host}:${puerto}/`);
});
```

Para ejecutar este servidor:

```bash
node servidor.js
```

Ahora puedes abrir tu navegador y visitar `http://localhost:3000` para ver el mensaje.

> **Nota**: El método `createServer()` recibe una función callback que se ejecuta cada vez que llega una solicitud al servidor. Los parámetros `req` (request) y `res` (response) representan la solicitud entrante y la respuesta que enviaremos, respectivamente.

## Manejo de archivos

Node.js proporciona el módulo `fs` (File System) para trabajar con archivos:

```javascript
// archivos.js
const fs = require('fs');

// Lectura de un archivo (forma asíncrona)
fs.readFile('ejemplo.txt', 'utf8', (err, datos) => {
  if (err) {
    console.error('Error al leer el archivo:', err);
    return;
  }
  console.log('Contenido del archivo:', datos);
});

// Escritura en un archivo (forma asíncrona)
const contenido = 'Este es el contenido que escribiremos en el archivo.';
fs.writeFile('nuevo.txt', contenido, (err) => {
  if (err) {
    console.error('Error al escribir el archivo:', err);
    return;
  }
  console.log('El archivo ha sido guardado correctamente');
});
```

> **Nota**: Las operaciones en Node.js son asíncronas por defecto, lo que significa que no bloquean la ejecución del programa mientras se completan. Por eso utilizamos callbacks para manejar el resultado una vez que la operación ha finalizado.

### Versión sincrónica (bloqueante)

También existen métodos síncronos, pero generalmente se recomienda usar los asíncronos para no bloquear el hilo principal:

```javascript
// Versión sincrónica (bloqueante)
try {
  const datos = fs.readFileSync('ejemplo.txt', 'utf8');
  console.log('Contenido del archivo (síncrono):', datos);
} catch (err) {
  console.error('Error al leer el archivo:', err);
}
```

> **Nota**: Las versiones síncronas de las funciones en Node.js terminan con "Sync" y bloquean la ejecución hasta que se completan. Esto puede afectar el rendimiento en aplicaciones con muchas operaciones de I/O.

## NPM: Gestión de paquetes

npm (Node Package Manager) es el gestor de paquetes que viene con Node.js y permite instalar y administrar bibliotecas de terceros.

### Iniciar un proyecto

Para iniciar un proyecto Node.js:

```bash
mkdir mi-proyecto
cd mi-proyecto
npm init -y
```

Esto crea un archivo `package.json` que contiene la configuración y dependencias de tu proyecto.

> **Nota**: El archivo `package.json` es fundamental en proyectos Node.js, ya que define metadatos como el nombre, versión, scripts y las dependencias del proyecto.

### Instalar paquetes

Para instalar un paquete (por ejemplo, Express, un framework web popular):

```bash
npm install express
```

Esto añadirá Express a las dependencias en tu `package.json` y lo descargará en la carpeta `node_modules`.

### Uso de paquetes instalados

Después de instalar un paquete, puedes usarlo en tu código:

```javascript
// app.js usando Express
const express = require('express');
const app = express();
const puerto = 3000;

app.get('/', (req, res) => {
  res.send('¡Hola mundo con Express!');
});

app.listen(puerto, () => {
  console.log(`Aplicación de ejemplo escuchando en http://localhost:${puerto}`);
});
```

> **Nota**: Express simplifica muchas tareas comunes en el desarrollo web con Node.js, como el enrutamiento, el manejo de peticiones y respuestas, y la integración de middleware.

## Programación asíncrona en Node.js

Node.js se basa en un modelo de programación asíncrona, lo que significa que las operaciones que pueden llevar tiempo (como leer archivos o hacer peticiones HTTP) no bloquean la ejecución del programa.

### Callbacks

Tradicionalmente, Node.js utilizaba callbacks para manejar operaciones asíncronas:

```javascript
// Ejemplo con callbacks
fs.readFile('archivo.txt', 'utf8', (err, datos) => {
  if (err) {
    console.error('Error:', err);
    return;
  }
  
  // Procesar los datos una vez que la lectura ha terminado
  console.log('Datos del archivo:', datos);
});

console.log('Este mensaje aparece antes que los datos del archivo');
```

> **Nota**: Los callbacks pueden llevar a lo que se conoce como "callback hell" o "pirámide de la perdición" cuando se anidan múltiples operaciones asíncronas, haciendo que el código sea difícil de leer y mantener.

### Promesas

Las versiones más recientes de Node.js incorporan soporte nativo para Promesas, que ofrecen una forma más elegante de manejar operaciones asíncronas:

```javascript
// Ejemplo con promesas
const fsPromises = require('fs').promises;

fsPromises.readFile('archivo.txt', 'utf8')
  .then(datos => {
    console.log('Datos del archivo:', datos);
  })
  .catch(err => {
    console.error('Error:', err);
  });

console.log('Este mensaje aparece antes que los datos del archivo');
```

> **Nota**: Las promesas representan un valor que puede estar disponible ahora, en el futuro, o nunca. Tienen tres estados posibles: pendiente, resuelta o rechazada.

### Async/Await

La sintaxis async/await, disponible desde Node.js 7.6, hace que el código asíncrono sea aún más legible:

```javascript
// Ejemplo con async/await
const fsPromises = require('fs').promises;

async function leerArchivo() {
  try {
    const datos = await fsPromises.readFile('archivo.txt', 'utf8');
    console.log('Datos del archivo:', datos);
  } catch (err) {
    console.error('Error:', err);
  }
}

leerArchivo();
console.log('Este mensaje aparece antes que los datos del archivo');
```

> **Nota**: `async/await` es "azúcar sintáctico" sobre las promesas, lo que significa que debajo utiliza promesas, pero proporciona una sintaxis más clara y similar a la programación síncrona tradicional.

## Eventos en Node.js

Node.js utiliza un patrón de diseño basado en eventos. El módulo `events` permite crear, emitir y escuchar eventos personalizados:

```javascript
// eventos.js
const EventEmitter = require('events');

// Crear una clase personalizada que herede de EventEmitter
class MiEmisor extends EventEmitter {}

// Crear una instancia
const emisor = new MiEmisor();

// Registrar un listener para el evento 'mensaje'
emisor.on('mensaje', (texto) => {
  console.log('Mensaje recibido:', texto);
});

// Emitir el evento 'mensaje'
emisor.emit('mensaje', '¡Hola! Este es un evento personalizado');
```

> **Nota**: Muchos módulos integrados de Node.js, como `http`, `fs` y `stream`, heredan del módulo `events` y utilizan este patrón de emisión y escucha de eventos.

## Streams (Flujos)

Los streams son una forma eficiente de manejar operaciones de lectura/escritura de datos, especialmente para archivos grandes o comunicación en red:

```javascript
// streams.js
const fs = require('fs');

// Stream de lectura
const streamLectura = fs.createReadStream('archivo-grande.txt', 'utf8');

// Stream de escritura
const streamEscritura = fs.createWriteStream('copia.txt');

// Escuchar eventos del stream de lectura
streamLectura.on('data', (chunk) => {
  console.log(`Recibido ${chunk.length} bytes de datos`);
});

streamLectura.on('end', () => {
  console.log('Lectura finalizada');
});

// Forma más eficiente: pipe (canalización)
// Conecta la salida del stream de lectura a la entrada del stream de escritura
streamLectura.pipe(streamEscritura);
```

> **Nota**: Los streams son especialmente útiles cuando trabajas con grandes cantidades de datos, ya que permiten procesarlos por fragmentos ("chunks") en lugar de cargar todo en memoria a la vez.

## Proyecto práctico: API REST básica

Vamos a crear una API REST simple que maneje una lista de tareas:

```javascript
// api-tareas.js
const express = require('express');
const app = express();
const puerto = 3000;

// Middleware para parsear JSON
app.use(express.json());

// Base de datos simulada (en memoria)
let tareas = [
  { id: 1, titulo: 'Aprender Node.js', completada: false },
  { id: 2, titulo: 'Crear una API REST', completada: false }
];

// Obtener todas las tareas
app.get('/tareas', (req, res) => {
  res.json(tareas);
});

// Obtener una tarea por ID
app.get('/tareas/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const tarea = tareas.find(t => t.id === id);
  
  if (!tarea) {
    return res.status(404).json({ mensaje: 'Tarea no encontrada' });
  }
  
  res.json(tarea);
});

// Crear una nueva tarea
app.post('/tareas', (req, res) => {
  const nuevaTarea = {
    id: tareas.length + 1,
    titulo: req.body.titulo,
    completada: false
  };
  
  tareas.push(nuevaTarea);
  res.status(201).json(nuevaTarea);
});

// Actualizar una tarea
app.put('/tareas/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const indice = tareas.findIndex(t => t.id === id);
  
  if (indice === -1) {
    return res.status(404).json({ mensaje: 'Tarea no encontrada' });
  }
  
  tareas[indice] = { ...tareas[indice], ...req.body };
  res.json(tareas[indice]);
});

// Eliminar una tarea
app.delete('/tareas/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const indice = tareas.findIndex(t => t.id === id);
  
  if (indice === -1) {
    return res.status(404).json({ mensaje: 'Tarea no encontrada' });
  }
  
  const tareaEliminada = tareas[indice];
  tareas = tareas.filter(t => t.id !== id);
  
  res.json(tareaEliminada);
});

// Iniciar el servidor
app.listen(puerto, () => {
  console.log(`API de tareas ejecutándose en http://localhost:${puerto}`);
});
```

Para probar esta API, puedes usar herramientas como [Postman](https://www.postman.com/) o [curl](https://curl.se/).

> **Nota**: Este es un ejemplo simplificado. En una aplicación real, deberías usar una base de datos persistente, implementar validación de datos, manejo de errores más robusto y posiblemente autenticación.

## Debugging en Node.js

Node.js ofrece varias opciones para depurar tu código:

### Console.log

El método más básico es usar `console.log()` para mostrar valores:

```javascript
console.log('Variable:', miVariable);
console.table(arrayDeObjetos); // Muestra arreglos o objetos en formato de tabla
console.time('operacion'); // Inicia un temporizador
// ... código a medir ...
console.timeEnd('operacion'); // Termina el temporizador y muestra el tiempo transcurrido
```

### Debugger incorporado

Node.js incluye un depurador que puedes activar con la palabra clave `debugger`:

```javascript
function sumar(a, b) {
  debugger; // El programa se detendrá aquí si se ejecuta con --inspect
  return a + b;
}
```

Para usar este depurador, ejecuta tu programa con:

```bash
node --inspect-brk archivo.js
```

Luego abre Chrome y visita `chrome://inspect` para conectarte al depurador.

> **Nota**: `--inspect-brk` detiene la ejecución al inicio del programa, mientras que `--inspect` solo se detiene en los puntos donde encuentre la palabra `debugger`.

## Mejores prácticas en Node.js

1. **Maneja errores adecuadamente**: Nunca ignores los errores, especialmente en callbacks y promesas.

```javascript
// Mal
fs.readFile('archivo.txt', (err, data) => {
  // ¡Error ignorado!
  console.log(data);
});

// Bien
fs.readFile('archivo.txt', (err, data) => {
  if (err) {
    console.error('Error:', err);
    return;
  }
  console.log(data);
});
```

2. **Usa la programación asíncrona correctamente**: Evita bloquear el hilo principal.

3. **Organiza tu código en módulos**: Mantén cada archivo enfocado en una responsabilidad específica.

4. **Usa un sistema de control de versiones** (como Git) para rastrear cambios.

5. **Implementa pruebas** para verificar que tu código funciona como se espera.

6. **Usa variables de entorno** para configuración (`process.env`):

```javascript
const puerto = process.env.PORT || 3000;
const modoProduccion = process.env.NODE_ENV === 'production';
```

7. **Manten tus dependencias actualizadas**, pero con precaución.

## Recursos adicionales

- [Documentación oficial de Node.js](https://nodejs.org/es/docs/)
- [MDN Web Docs - Node.js](https://developer.mozilla.org/es/docs/Learn/Server-side/Express_Nodejs)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
