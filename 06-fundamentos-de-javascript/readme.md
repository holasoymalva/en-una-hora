# Fundamentos de JavaScript - Guía de 1 Hora

Aprenderás los conceptos fundamentales de JavaScript en solo 1 hora! 🚀

## Requisitos Previos
- Un editor de código (VS Code recomendado)
- Node.js instalado o un navegador web moderno
- ¡Ganas de aprender!

## 1. Variables y Tipos de Datos (8 minutos)

```javascript
// Variables: let, const, var
let edad = 25;                // Variable que puede cambiar
const nombre = "Juan";        // Constante, no puede cambiar
var antiguaForma = "evitar";  // Forma antigua, mejor usar let o const

// Tipos de datos
let numero = 42;              // Number
let texto = "Hola mundo";     // String
let booleano = true;          // Boolean
let nulo = null;              // Null
let indefinido = undefined;   // Undefined
let arreglo = [1, 2, 3];     // Array
let objeto = {               // Object
    nombre: "Ana",
    edad: 30
};

// Template Strings
let saludo = `Hola ${nombre}, tienes ${edad} años`;
console.log(saludo);
```

## 2. Operadores (7 minutos)

```javascript
// Aritméticos
let suma = 5 + 3;          // 8
let resta = 10 - 4;        // 6
let multiplicacion = 3 * 4; // 12
let division = 15 / 3;     // 5
let modulo = 17 % 5;       // 2
let exponente = 2 ** 3;    // 8

// Comparación
let mayor = 5 > 3;         // true
let menor = 5 < 3;         // false
let igual = 5 == "5";      // true (comparación no estricta)
let estricto = 5 === "5";  // false (comparación estricta)
let diferente = 5 != "5";  // false
let estrictoDif = 5 !== "5"; // true

// Lógicos
let and = true && false;   // false
let or = true || false;    // true
let not = !true;          // false
```

## 3. Control de Flujo (10 minutos)

```javascript
// If, else if, else
let hora = 14;

if (hora < 12) {
    console.log("Buenos días");
} else if (hora < 18) {
    console.log("Buenas tardes");
} else {
    console.log("Buenas noches");
}

// Switch
let dia = "Lunes";
switch (dia) {
    case "Lunes":
        console.log("Inicio de semana");
        break;
    case "Viernes":
        console.log("¡Fin de semana!");
        break;
    default:
        console.log("Otro día");
}

// Operador ternario
let edad = 20;
let mensaje = edad >= 18 ? "Eres mayor de edad" : "Eres menor de edad";
```

## 4. Bucles (10 minutos)

```javascript
// For
for (let i = 0; i < 5; i++) {
    console.log(i); // 0, 1, 2, 3, 4
}

// While
let contador = 0;
while (contador < 3) {
    console.log(contador); // 0, 1, 2
    contador++;
}

// Do while
let num = 0;
do {
    console.log(num); // Se ejecuta al menos una vez
    num++;
} while (num < 2);

// For...of (para arrays)
let frutas = ["manzana", "pera", "uva"];
for (let fruta of frutas) {
    console.log(fruta);
}

// For...in (para objetos)
let persona = {
    nombre: "María",
    edad: 25,
    ciudad: "Madrid"
};
for (let propiedad in persona) {
    console.log(`${propiedad}: ${persona[propiedad]}`);
}
```

## 5. Funciones (10 minutos)

```javascript
// Función básica
function saludar(nombre) {
    return `Hola ${nombre}!`;
}

// Función flecha
const sumar = (a, b) => a + b;

// Función con parámetros por defecto
function configurar(opciones = { color: "rojo", tamaño: "mediano" }) {
    console.log(opciones);
}

// Callback
function procesarUsuario(nombre, callback) {
    let usuario = { nombre: nombre };
    callback(usuario);
}

procesarUsuario("Ana", usuario => {
    console.log(`Procesando usuario: ${usuario.nombre}`);
});

// Scope y Closure
function crearContador() {
    let contador = 0;
    return function() {
        return ++contador;
    };
}

const contar = crearContador();
console.log(contar()); // 1
console.log(contar()); // 2
```

## 6. Arrays y Métodos (8 minutos)

```javascript
// Creación y manipulación
let numeros = [1, 2, 3, 4, 5];
numeros.push(6);         // Añadir al final
numeros.pop();          // Eliminar del final
numeros.unshift(0);     // Añadir al inicio
numeros.shift();        // Eliminar del inicio

// Métodos útiles
let nums = [1, 2, 3, 4, 5];

// map - transformar elementos
let duplicados = nums.map(x => x * 2);

// filter - filtrar elementos
let pares = nums.filter(x => x % 2 === 0);

// reduce - reducir a un valor
let suma = nums.reduce((acc, curr) => acc + curr, 0);

// find - encontrar elemento
let encontrado = nums.find(x => x > 3);

// forEach - iterar
nums.forEach(x => console.log(x));
```

## 7. Objetos y Métodos (7 minutos)

```javascript
// Creación de objeto
let persona = {
    nombre: "Carlos",
    edad: 30,
    saludar() {
        return `Hola, soy ${this.nombre}`;
    }
};

// Desestructuración
let { nombre, edad } = persona;

// Spread operator
let personaExtendida = {
    ...persona,
    ciudad: "Barcelona"
};

// Object methods
console.log(Object.keys(persona));
console.log(Object.values(persona));
console.log(Object.entries(persona));
```

## Ejercicios Prácticos

1. Crear una función que calcule el promedio de un array de números
```javascript
function promedio(numeros) {
    return numeros.reduce((acc, curr) => acc + curr, 0) / numeros.length;
}
```

2. Crear un objeto "banco" con métodos para depositar y retirar dinero
```javascript
const cuenta = {
    saldo: 0,
    depositar(cantidad) {
        this.saldo += cantidad;
        return `Nuevo saldo: ${this.saldo}`;
    },
    retirar(cantidad) {
        if (cantidad <= this.saldo) {
            this.saldo -= cantidad;
            return `Nuevo saldo: ${this.saldo}`;
        }
        return "Saldo insuficiente";
    }
};
```

## Recursos Adicionales
- [MDN JavaScript Guide](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide)
- [JavaScript.info](https://javascript.info/)
- [Eloquent JavaScript](https://eloquentjavascript.net/)
- [FreeCodeCamp JavaScript Course](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)

¡Felicitaciones! 🎉 Has completado el tutorial de fundamentos de JavaScript. Ahora tienes las bases necesarias para comenzar a programar en JavaScript.
