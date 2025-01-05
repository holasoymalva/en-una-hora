# Asincronía en JavaScript - Guía de 30 Minutos

Aprenderás los conceptos fundamentales de la programación asíncrona en JavaScript en solo 30 minutos! 🚀

## Requisitos Previos
- Conocimientos básicos de JavaScript
- Un editor de código (VS Code recomendado)
- Node.js instalado

## 1. Callbacks (7 minutos)

```javascript
// Ejemplo simple de callback
console.log('Inicio');

setTimeout(() => {
    console.log('Ejecutado después de 2 segundos');
}, 2000);

console.log('Fin');

// Ejemplo práctico de callback
function obtenerUsuario(id, callback) {
    setTimeout(() => {
        const usuario = {
            id: id,
            nombre: 'Juan',
            email: 'juan@ejemplo.com'
        };
        callback(usuario);
    }, 1000);
}

// Callback Hell (ejemplo de lo que queremos evitar)
obtenerUsuario(1, (usuario) => {
    console.log(usuario);
    obtenerPosts(usuario.id, (posts) => {
        console.log(posts);
        obtenerComentarios(posts[0].id, (comentarios) => {
            console.log(comentarios);
            // Y así sucesivamente...
        });
    });
});
```

> **Nota sobre Callbacks:**
> - Un callback es una función que se pasa como argumento a otra función
> - Se ejecuta después de que la función principal ha terminado
> - Pueden llevar a "callback hell" cuando se anidan múltiples callbacks
> - Fueron la primera forma de manejar asincronía en JavaScript
> - Son la base para entender las promesas y async/await

## 2. Promesas (8 minutos)

```javascript
// Crear una promesa
const miPromesa = new Promise((resolve, reject) => {
    const exito = true;
    
    if (exito) {
        resolve('¡Operación exitosa!');
    } else {
        reject('¡Hubo un error!');
    }
});

// Usar la promesa
miPromesa
    .then(resultado => console.log(resultado))
    .catch(error => console.error(error));

// Ejemplo práctico: obtener datos
function obtenerUsuarioPromesa(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const usuario = {
                id: id,
                nombre: 'Juan',
                email: 'juan@ejemplo.com'
            };
            resolve(usuario);
        }, 1000);
    });
}

// Encadenar promesas
obtenerUsuarioPromesa(1)
    .then(usuario => obtenerPostsPromesa(usuario.id))
    .then(posts => obtenerComentariosPromesa(posts[0].id))
    .then(comentarios => console.log(comentarios))
    .catch(error => console.error('Error:', error));

// Promise.all - ejecutar promesas en paralelo
const promesa1 = Promise.resolve(3);
const promesa2 = new Promise(resolve => setTimeout(() => resolve('foo'), 2000));
const promesa3 = Promise.resolve('bar');

Promise.all([promesa1, promesa2, promesa3])
    .then(valores => console.log(valores)); // [3, 'foo', 'bar']
```

> **Nota sobre Promesas:**
> - Una promesa representa un valor que estará disponible en el futuro
> - Tiene tres estados: pending, fulfilled (resuelta), rejected (rechazada)
> - `.then()` maneja el caso de éxito
> - `.catch()` maneja los errores
> - Se pueden encadenar con `.then()`
> - `Promise.all()` ejecuta múltiples promesas en paralelo
> - Resuelven el problema del "callback hell"

## 3. Async/Await (8 minutos)

```javascript
// Función asíncrona básica
async function obtenerDatos() {
    try {
        const usuario = await obtenerUsuarioPromesa(1);
        console.log(usuario);
        
        const posts = await obtenerPostsPromesa(usuario.id);
        console.log(posts);
        
        const comentarios = await obtenerComentariosPromesa(posts[0].id);
        console.log(comentarios);
        
    } catch (error) {
        console.error('Error:', error);
    }
}

// Ejemplo práctico con fetch
async function obtenerPokemon(id) {
    try {
        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error al obtener Pokemon:', error);
        throw error;
    }
}

// Función asíncrona con Promise.all
async function obtenerVariosPokemons() {
    try {
        const ids = [1, 2, 3];
        const promesas = ids.map(id => obtenerPokemon(id));
        const pokemons = await Promise.all(promesas);
        console.log(pokemons);
    } catch (error) {
        console.error('Error:', error);
    }
}
```

> **Nota sobre Async/Await:**
> - `async` marca una función como asíncrona
> - `await` pausa la ejecución hasta que la promesa se resuelva
> - Hace que el código asíncrono parezca síncrono
> - Siempre devuelve una promesa
> - Debe usarse try/catch para manejar errores
> - Se puede usar con Promise.all para operaciones en paralelo

## 4. Ejemplo Práctico Completo (7 minutos)

```javascript
// Simulación de base de datos
const DB = {
    usuarios: [
        { id: 1, nombre: 'Juan', email: 'juan@ejemplo.com' },
        { id: 2, nombre: 'María', email: 'maria@ejemplo.com' }
    ],
    posts: [
        { id: 1, userId: 1, titulo: 'Post 1' },
        { id: 2, userId: 1, titulo: 'Post 2' },
        { id: 3, userId: 2, titulo: 'Post 3' }
    ],
    comentarios: [
        { id: 1, postId: 1, texto: 'Comentario 1' },
        { id: 2, postId: 1, texto: 'Comentario 2' },
        { id: 3, postId: 2, texto: 'Comentario 3' }
    ]
};

// Funciones de la API
function obtenerUsuario(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const usuario = DB.usuarios.find(u => u.id === id);
            if (usuario) {
                resolve(usuario);
            } else {
                reject(new Error('Usuario no encontrado'));
            }
        }, 1000);
    });
}

function obtenerPosts(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const posts = DB.posts.filter(p => p.userId === userId);
            if (posts.length > 0) {
                resolve(posts);
            } else {
                reject(new Error('Posts no encontrados'));
            }
        }, 1000);
    });
}

function obtenerComentarios(postId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const comentarios = DB.comentarios.filter(c => c.postId === postId);
            if (comentarios.length > 0) {
                resolve(comentarios);
            } else {
                reject(new Error('Comentarios no encontrados'));
            }
        }, 1000);
    });
}

// Uso con async/await
async function obtenerDatosCompletos(userId) {
    try {
        // Obtener usuario
        const usuario = await obtenerUsuario(userId);
        console.log('Usuario:', usuario);

        // Obtener posts del usuario
        const posts = await obtenerPosts(usuario.id);
        console.log('Posts:', posts);

        // Obtener comentarios del primer post
        const comentarios = await obtenerComentarios(posts[0].id);
        console.log('Comentarios:', comentarios);

        return {
            usuario,
            posts,
            comentarios
        };
    } catch (error) {
        console.error('Error en la operación:', error);
        throw error;
    }
}

// Ejecutar el ejemplo
obtenerDatosCompletos(1)
    .then(resultado => console.log('Resultado final:', resultado))
    .catch(error => console.error('Error final:', error));
```

## Recursos Adicionales
- [MDN - Asincronía en JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [JavaScript.info - Async/Await](https://javascript.info/async-await)
- [WesBos - JavaScript30](https://javascript30.com/)

¡Felicitaciones! 🎉 Has completado el tutorial de asincronía en JavaScript. Ahora tienes las bases necesarias para manejar operaciones asíncronas de manera efectiva.
