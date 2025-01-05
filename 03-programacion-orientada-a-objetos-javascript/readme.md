# Programación Orientada a Objetos en JavaScript - Guía de 40 Minutos

Aprenderás los conceptos fundamentales de la Programación Orientada a Objetos (POO) en JavaScript de manera práctica y sencilla! 🚀

## Requisitos Previos
- Conocimientos básicos de JavaScript
- Un editor de código (VS Code recomendado)
- Node.js instalado (opcional, para ejecutar ejemplos)

## 1. Clases y Objetos (8 minutos)

```javascript
// Definición de una clase básica
class Persona {
    // Constructor
    constructor(nombre, edad) {
        this.nombre = nombre;  // Propiedad pública
        this.edad = edad;      // Propiedad pública
    }
    
    // Método
    saludar() {
        return `¡Hola! Me llamo ${this.nombre} y tengo ${this.edad} años`;
    }

    // Método estático (pertenece a la clase, no a las instancias)
    static crearAdulto(nombre) {
        return new Persona(nombre, 18);
    }
}

// Crear objetos (instancias)
const persona1 = new Persona("Ana", 25);
const persona2 = new Persona("Juan", 30);
const adulto = Persona.crearAdulto("Carlos");

console.log(persona1.saludar());  // ¡Hola! Me llamo Ana y tengo 25 años
console.log(persona2.nombre);     // Juan
```

> **Nota sobre Clases:**
> - Las clases son "plantillas" para crear objetos
> - `constructor` es el método que se llama al crear una instancia
> - `this` se refiere a la instancia actual del objeto
> - Los métodos son funciones que pertenecen a la clase
> - Los métodos estáticos se llaman en la clase, no en las instancias

## 2. Encapsulamiento (8 minutos)

```javascript
// Encapsulamiento usando closures y convenciones
class CuentaBancaria {
    #saldo; // Propiedad privada (ES2022+)
    
    constructor(titular, saldoInicial) {
        this.titular = titular;
        this.#saldo = saldoInicial;
        this._movimientos = []; // Convención para "protegido"
    }
    
    // Getter
    get saldo() {
        return this.#saldo;
    }
    
    // Setter
    set saldo(nuevoSaldo) {
        if (nuevoSaldo >= 0) {
            this.#saldo = nuevoSaldo;
        } else {
            throw new Error("El saldo no puede ser negativo");
        }
    }
    
    depositar(cantidad) {
        if (cantidad > 0) {
            this.#saldo += cantidad;
            this._registrarMovimiento('depósito', cantidad);
            return `Depósito de ${cantidad} realizado. Nuevo saldo: ${this.#saldo}`;
        }
        return "La cantidad debe ser positiva";
    }
    
    retirar(cantidad) {
        if (cantidad > 0 && cantidad <= this.#saldo) {
            this.#saldo -= cantidad;
            this._registrarMovimiento('retiro', cantidad);
            return `Retiro de ${cantidad} realizado. Nuevo saldo: ${this.#saldo}`;
        }
        return "Fondos insuficientes o cantidad inválida";
    }
    
    // Método "protegido"
    _registrarMovimiento(tipo, cantidad) {
        this._movimientos.push({
            tipo,
            cantidad,
            fecha: new Date()
        });
    }
}

// Uso de la clase
const cuenta = new CuentaBancaria("Juan Pérez", 1000);
console.log(cuenta.saldo);        // Acceso mediante getter
cuenta.saldo = 1500;             // Uso del setter
console.log(cuenta.depositar(500));
console.log(cuenta.retirar(200));
```

> **Nota sobre Encapsulamiento:**
> - `#propiedad`: Declara una propiedad privada (nueva sintaxis)
> - `_propiedad`: Convención para indicar que es "protegido"
> - Getters y setters controlan el acceso a las propiedades
> - El encapsulamiento ayuda a mantener la integridad de los datos

## 3. Herencia (8 minutos)

```javascript
class Animal {
    constructor(nombre, edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    hacerSonido() {
        return "Algún sonido";
    }
    
    obtenerInfo() {
        return `${this.nombre} tiene ${this.edad} años`;
    }
}

class Perro extends Animal {
    constructor(nombre, edad, raza) {
        super(nombre, edad); // Llamar al constructor padre
        this.raza = raza;
    }
    
    hacerSonido() { // Sobrescribir método
        return "¡Guau!";
    }
    
    obtenerInfo() {
        return `${super.obtenerInfo()} y es un ${this.raza}`;
    }
}

class Gato extends Animal {
    hacerSonido() {
        return "¡Miau!";
    }
}

// Uso de las clases
const perro = new Perro("Max", 3, "Labrador");
const gato = new Gato("Luna", 2);

console.log(perro.obtenerInfo());   // Max tiene 3 años y es un Labrador
console.log(perro.hacerSonido());   // ¡Guau!
console.log(gato.hacerSonido());    // ¡Miau!
```

> **Nota sobre Herencia:**
> - `extends` indica que una clase hereda de otra
> - `super()` llama al constructor de la clase padre
> - Se pueden sobrescribir métodos del padre
> - `super.metodo()` llama a un método del padre

## 4. Polimorfismo (8 minutos)

```javascript
class Forma {
    area() {
        return 0;
    }
    
    obtenerInfo() {
        return `Área: ${this.area()}`;
    }
}

class Rectangulo extends Forma {
    constructor(base, altura) {
        super();
        this.base = base;
        this.altura = altura;
    }
    
    area() {
        return this.base * this.altura;
    }
}

class Circulo extends Forma {
    constructor(radio) {
        super();
        this.radio = radio;
    }
    
    area() {
        return Math.PI * this.radio ** 2;
    }
}

// Polimorfismo en acción
function mostrarArea(forma) {
    console.log(forma.obtenerInfo());
}

// Uso del polimorfismo
const rectangulo = new Rectangulo(5, 3);
const circulo = new Circulo(4);

mostrarArea(rectangulo);  // Funciona con cualquier forma
mostrarArea(circulo);     // Funciona con cualquier forma

// Array de formas diferentes
const formas = [
    new Rectangulo(4, 5),
    new Circulo(3),
    new Rectangulo(2, 6)
];

// Tratar todas las formas de manera uniforme
formas.forEach(forma => {
    console.log(forma.obtenerInfo());
});
```

## 5. Ejemplo Práctico Completo (8 minutos)

```javascript
// Sistema de Gestión de Biblioteca
class Libro {
    #disponible;
    
    constructor(titulo, autor, isbn) {
        this.titulo = titulo;
        this.autor = autor;
        this.isbn = isbn;
        this.#disponible = true;
    }
    
    get disponible() {
        return this.#disponible;
    }
    
    prestar() {
        if (this.#disponible) {
            this.#disponible = false;
            return true;
        }
        return false;
    }
    
    devolver() {
        this.#disponible = true;
    }
    
    obtenerInfo() {
        return `${this.titulo} por ${this.autor} (ISBN: ${this.isbn}) - ${this.#disponible ? 'Disponible' : 'Prestado'}`;
    }
}

class Usuario {
    constructor(nombre, id) {
        this.nombre = nombre;
        this.id = id;
        this.librosPrestados = [];
    }
    
    tomarPrestado(libro) {
        if (libro.prestar()) {
            this.librosPrestados.push(libro);
            return true;
        }
        return false;
    }
    
    devolverLibro(libro) {
        const index = this.librosPrestados.indexOf(libro);
        if (index !== -1) {
            libro.devolver();
            this.librosPrestados.splice(index, 1);
            return true;
        }
        return false;
    }
    
    listarLibros() {
        return this.librosPrestados.map(libro => libro.obtenerInfo());
    }
}

class Biblioteca {
    constructor(nombre) {
        this.nombre = nombre;
        this.libros = [];
        this.usuarios = [];
    }
    
    agregarLibro(libro) {
        this.libros.push(libro);
    }
    
    registrarUsuario(usuario) {
        this.usuarios.push(usuario);
    }
    
    buscarLibro(isbn) {
        return this.libros.find(libro => libro.isbn === isbn);
    }
    
    listarLibrosDisponibles() {
        return this.libros
            .filter(libro => libro.disponible)
            .map(libro => libro.obtenerInfo());
    }
}

// Uso del sistema
const biblioteca = new Biblioteca("Biblioteca Central");

// Crear algunos libros
const libro1 = new Libro("El Señor de los Anillos", "J.R.R. Tolkien", "123");
const libro2 = new Libro("Fundación", "Isaac Asimov", "456");
biblioteca.agregarLibro(libro1);
biblioteca.agregarLibro(libro2);

// Crear un usuario
const usuario = new Usuario("Ana García", "U001");
biblioteca.registrarUsuario(usuario);

// Realizar préstamos
console.log(biblioteca.listarLibrosDisponibles());
console.log(usuario.tomarPrestado(libro1) ? "Préstamo exitoso" : "Libro no disponible");
console.log(biblioteca.listarLibrosDisponibles());
console.log(usuario.listarLibros());
```

## Recursos Adicionales
- [MDN Web Docs - POO en JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Classes)
- [JavaScript.info - Clases](https://javascript.info/classes)
- [Patrones de Diseño en JavaScript](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)

¡Felicitaciones! 🎉 Has completado el tutorial de Programación Orientada a Objetos en JavaScript. Ahora tienes las bases necesarias para crear programas usando POO.
