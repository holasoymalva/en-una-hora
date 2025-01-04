# Estructuras de Datos en JavaScript - Guía de 1 Hora

Aprenderás las estructuras de datos fundamentales implementadas en JavaScript en solo 1 hora! 🚀

## Requisitos Previos
- Conocimientos básicos de JavaScript
- Comprensión de clases y objetos
- Un editor de código (VS Code recomendado)
- Node.js instalado (para ejecutar los ejemplos)

## 1. Arrays y Métodos (8 minutos)
```javascript
// Arrays y sus métodos principales
const array = [1, 2, 3, 4, 5];

// Añadir elementos
array.push(6);        // Al final
array.unshift(0);     // Al inicio

// Eliminar elementos
array.pop();          // Del final
array.shift();        // Del inicio

// Slice y Splice
const subArray = array.slice(1, 3);  // Copia parte del array
array.splice(1, 2, 'a', 'b');        // Modifica el array original

// Ejemplo práctico: Filtrar números pares
const numeros = [1, 2, 3, 4, 5, 6, 7, 8];
const pares = numeros.filter(num => num % 2 === 0);
console.log(pares); // [2, 4, 6, 8]
```

## 2. Pila (Stack) (8 minutos)
```javascript
class Pila {
    constructor() {
        this.items = [];
    }

    // Agregar elemento al tope
    push(elemento) {
        this.items.push(elemento);
    }

    // Remover y retornar elemento del tope
    pop() {
        if (this.isEmpty()) {
            return "La pila está vacía";
        }
        return this.items.pop();
    }

    // Ver elemento del tope
    peek() {
        if (this.isEmpty()) {
            return "La pila está vacía";
        }
        return this.items[this.items.length - 1];
    }

    isEmpty() {
        return this.items.length === 0;
    }

    size() {
        return this.items.length;
    }

    clear() {
        this.items = [];
    }
}

// Ejemplo de uso
const pila = new Pila();
pila.push("A");
pila.push("B");
pila.push("C");
console.log(pila.pop()); // "C"
console.log(pila.peek()); // "B"
```

## 3. Cola (Queue) (8 minutos)
```javascript
class Cola {
    constructor() {
        this.items = {};
        this.frontIndex = 0;
        this.backIndex = 0;
    }

    // Agregar elemento al final
    enqueue(elemento) {
        this.items[this.backIndex] = elemento;
        this.backIndex++;
    }

    // Remover y retornar elemento del frente
    dequeue() {
        if (this.isEmpty()) {
            return "La cola está vacía";
        }
        const item = this.items[this.frontIndex];
        delete this.items[this.frontIndex];
        this.frontIndex++;
        return item;
    }

    // Ver elemento del frente
    front() {
        if (this.isEmpty()) {
            return "La cola está vacía";
        }
        return this.items[this.frontIndex];
    }

    isEmpty() {
        return this.frontIndex === this.backIndex;
    }

    size() {
        return this.backIndex - this.frontIndex;
    }
}

// Ejemplo de uso
const cola = new Cola();
cola.enqueue("Primero");
cola.enqueue("Segundo");
console.log(cola.dequeue()); // "Primero"
console.log(cola.front()); // "Segundo"
```

## 4. Lista Enlazada (12 minutos)
```javascript
class Nodo {
    constructor(valor) {
        this.valor = valor;
        this.siguiente = null;
    }
}

class ListaEnlazada {
    constructor() {
        this.cabeza = null;
        this.longitud = 0;
    }

    // Agregar al final
    append(valor) {
        const nuevoNodo = new Nodo(valor);
        
        if (!this.cabeza) {
            this.cabeza = nuevoNodo;
        } else {
            let actual = this.cabeza;
            while (actual.siguiente) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevoNodo;
        }
        this.longitud++;
    }

    // Agregar al inicio
    prepend(valor) {
        const nuevoNodo = new Nodo(valor);
        nuevoNodo.siguiente = this.cabeza;
        this.cabeza = nuevoNodo;
        this.longitud++;
    }

    // Eliminar valor
    delete(valor) {
        if (!this.cabeza) return;

        if (this.cabeza.valor === valor) {
            this.cabeza = this.cabeza.siguiente;
            this.longitud--;
            return;
        }

        let actual = this.cabeza;
        while (actual.siguiente) {
            if (actual.siguiente.valor === valor) {
                actual.siguiente = actual.siguiente.siguiente;
                this.longitud--;
                return;
            }
            actual = actual.siguiente;
        }
    }

    // Imprimir lista
    print() {
        const valores = [];
        let actual = this.cabeza;
        while (actual) {
            valores.push(actual.valor);
            actual = actual.siguiente;
        }
        return valores;
    }
}

// Ejemplo de uso
const lista = new ListaEnlazada();
lista.append(1);
lista.append(2);
lista.prepend(0);
console.log(lista.print()); // [0, 1, 2]
```

## 5. Árbol Binario (12 minutos)
```javascript
class NodoArbol {
    constructor(valor) {
        this.valor = valor;
        this.izquierda = null;
        this.derecha = null;
    }
}

class ArbolBinario {
    constructor() {
        this.raiz = null;
    }

    insertar(valor) {
        const nuevoNodo = new NodoArbol(valor);

        if (!this.raiz) {
            this.raiz = nuevoNodo;
            return;
        }

        this.insertarNodo(this.raiz, nuevoNodo);
    }

    insertarNodo(nodo, nuevoNodo) {
        if (nuevoNodo.valor < nodo.valor) {
            if (!nodo.izquierda) {
                nodo.izquierda = nuevoNodo;
            } else {
                this.insertarNodo(nodo.izquierda, nuevoNodo);
            }
        } else {
            if (!nodo.derecha) {
                nodo.derecha = nuevoNodo;
            } else {
                this.insertarNodo(nodo.derecha, nuevoNodo);
            }
        }
    }

    // Recorrido inorden
    inorden(nodo = this.raiz) {
        if (!nodo) return [];
        return [
            ...this.inorden(nodo.izquierda),
            nodo.valor,
            ...this.inorden(nodo.derecha)
        ];
    }

    // Búsqueda
    buscar(valor, nodo = this.raiz) {
        if (!nodo || nodo.valor === valor) {
            return nodo;
        }

        if (valor < nodo.valor) {
            return this.buscar(valor, nodo.izquierda);
        }
        return this.buscar(valor, nodo.derecha);
    }
}

// Ejemplo de uso
const arbol = new ArbolBinario();
arbol.insertar(5);
arbol.insertar(3);
arbol.insertar(7);
console.log(arbol.inorden()); // [3, 5, 7]
```

## 6. Tabla Hash (Mapa) (12 minutos)
```javascript
class TablaHash {
    constructor(size = 53) {
        this.keyMap = new Array(size);
    }

    _hash(key) {
        let total = 0;
        const PRIMO = 31;
        for (let i = 0; i < Math.min(key.length, 100); i++) {
            const char = key[i];
            const value = char.charCodeAt(0) - 96;
            total = (total * PRIMO + value) % this.keyMap.length;
        }
        return total;
    }

    set(key, value) {
        const index = this._hash(key);
        if (!this.keyMap[index]) {
            this.keyMap[index] = [];
        }
        this.keyMap[index].push([key, value]);
    }

    get(key) {
        const index = this._hash(key);
        if (this.keyMap[index]) {
            for (let i = 0; i < this.keyMap[index].length; i++) {
                if (this.keyMap[index][i][0] === key) {
                    return this.keyMap[index][i][1];
                }
            }
        }
        return undefined;
    }

    keys() {
        const keysArr = [];
        for (let i = 0; i < this.keyMap.length; i++) {
            if (this.keyMap[i]) {
                for (let j = 0; j < this.keyMap[i].length; j++) {
                    keysArr.push(this.keyMap[i][j][0]);
                }
            }
        }
        return keysArr;
    }
}

// Ejemplo de uso
const tabla = new TablaHash(17);
tabla.set("rojo", "#ff0000");
tabla.set("azul", "#0000ff");
console.log(tabla.get("rojo")); // "#ff0000"
```

## Ejercicios Prácticos

1. Implementa una cola de prioridad
2. Crea una lista doblemente enlazada
3. Implementa un árbol binario de búsqueda balanceado
4. Crea un grafo y sus operaciones básicas
5. Implementa una tabla hash con manejo de colisiones

## Recursos Adicionales
- [MDN Web Docs](https://developer.mozilla.org/es/docs/Web/JavaScript)
- [JavaScript Data Structures Repository](https://github.com/trekhleb/javascript-algorithms)
- [VisuAlgo](https://visualgo.net/) - Visualización de estructuras de datos
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)

¡Felicitaciones! 🎉 Has completado el tutorial básico de estructuras de datos en JavaScript. Ahora tienes las bases necesarias para implementar y utilizar las estructuras de datos más comunes.
