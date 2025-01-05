# Algoritmos y Complejidad - Guía de 1 Hora

Aprenderás los conceptos fundamentales de algoritmos y complejidad computacional usando JavaScript en solo 1 hora! 🚀

## Requisitos Previos
- Conocimientos básicos de JavaScript
- Un editor de código (VS Code recomendado)
- Node.js instalado

## 1. Introducción a la Complejidad (8 minutos)

### Notación Big O
La notación Big O describe el rendimiento o complejidad de un algoritmo.

Complejidades comunes:
- O(1) - Constante
- O(log n) - Logarítmica
- O(n) - Lineal
- O(n log n) - Linearítmica
- O(n²) - Cuadrática
- O(2ⁿ) - Exponencial

## 2. Complejidad Constante O(1) (5 minutos)

```javascript
// O(1) - Acceso a array por índice
function obtenerPrimerElemento(arr) {
    return arr[0];  // Siempre toma el mismo tiempo sin importar el tamaño del array
}

// O(1) - Operaciones matemáticas simples
function sumarDosNumeros(a, b) {
    return a + b;  // Tiempo constante
}
```

## 3. Complejidad Lineal O(n) (10 minutos)

```javascript
// O(n) - Búsqueda lineal
function buscarElemento(arr, elemento) {
    console.time('busquedaLineal');
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === elemento) {
            console.timeEnd('busquedaLineal');
            return i;
        }
    }
    console.timeEnd('busquedaLineal');
    return -1;
}

// O(n) - Suma de elementos
function sumarElementos(arr) {
    let suma = 0;
    for (let elemento of arr) {
        suma += elemento;
    }
    return suma;
}

// Ejemplo de uso y medición
const arr = Array.from({length: 10000}, (_, i) => i);
console.log(buscarElemento(arr, 9999));  // Peor caso
console.log(buscarElemento(arr, 50));    // Caso promedio
```

## 4. Complejidad Cuadrática O(n²) (12 minutos)

```javascript
// O(n²) - Bubble Sort
function bubbleSort(arr) {
    console.time('bubbleSort');
    const n = arr.length;
    
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Intercambiar elementos
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
    
    console.timeEnd('bubbleSort');
    return arr;
}

// O(n²) - Encontrar todos los pares de suma
function encontrarParesSuma(arr, objetivo) {
    const pares = [];
    
    for (let i = 0; i < arr.length; i++) {
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[i] + arr[j] === objetivo) {
                pares.push([arr[i], arr[j]]);
            }
        }
    }
    
    return pares;
}
```

## 5. Complejidad Logarítmica O(log n) (10 minutos)

```javascript
// O(log n) - Búsqueda binaria
function busquedaBinaria(arr, objetivo) {
    console.time('busquedaBinaria');
    let inicio = 0;
    let fin = arr.length - 1;

    while (inicio <= fin) {
        let medio = Math.floor((inicio + fin) / 2);

        if (arr[medio] === objetivo) {
            console.timeEnd('busquedaBinaria');
            return medio;
        }
        
        if (arr[medio] < objetivo) {
            inicio = medio + 1;
        } else {
            fin = medio - 1;
        }
    }

    console.timeEnd('busquedaBinaria');
    return -1;
}

// Comparación con búsqueda lineal
const arrOrdenado = Array.from({length: 1000000}, (_, i) => i);
console.log("Búsqueda Binaria:");
busquedaBinaria(arrOrdenado, 999999);
console.log("\nBúsqueda Lineal:");
buscarElemento(arrOrdenado, 999999);
```

## 6. Complejidad O(n log n) (10 minutos)

```javascript
// O(n log n) - Merge Sort
function mergeSort(arr) {
    if (arr.length <= 1) return arr;

    const medio = Math.floor(arr.length / 2);
    const izquierda = arr.slice(0, medio);
    const derecha = arr.slice(medio);

    return merge(mergeSort(izquierda), mergeSort(derecha));
}

function merge(izq, der) {
    let resultado = [];
    let i = 0;
    let j = 0;

    while (i < izq.length && j < der.length) {
        if (izq[i] < der[j]) {
            resultado.push(izq[i]);
            i++;
        } else {
            resultado.push(der[j]);
            j++;
        }
    }

    return resultado.concat(izq.slice(i)).concat(der.slice(j));
}

// Comparación de algoritmos de ordenamiento
const arrDesordenado = Array.from({length: 10000}, () => Math.floor(Math.random() * 10000));

console.time('mergeSort');
const arrMergeSort = mergeSort([...arrDesordenado]);
console.timeEnd('mergeSort');

console.time('bubbleSort');
const arrBubbleSort = bubbleSort([...arrDesordenado]);
console.timeEnd('bubbleSort');
```

## 7. Optimización y Espacio (5 minutos)

### Tips de Optimización
1. Evitar bucles anidados innecesarios
2. Usar estructuras de datos apropiadas
3. Considerar tanto tiempo como espacio
4. Memoización para problemas recursivos

```javascript
// Ejemplo de memoización
const fibonacci = (function() {
    const memo = {};
    
    return function fib(n) {
        if (n in memo) return memo[n];
        if (n <= 1) return n;
        
        return memo[n] = fib(n - 1) + fib(n - 2);
    }
})();

// Comparar rendimiento
console.time('fibonacci');
console.log(fibonacci(40));
console.timeEnd('fibonacci');
```

## Ejercicios Prácticos

1. Implementa una función que encuentre el elemento que más se repite en un array
2. Crea un algoritmo de ordenamiento por selección y compara su rendimiento
3. Optimiza un algoritmo O(n²) para hacerlo O(n)
4. Implementa una búsqueda binaria recursiva

## Recursos Adicionales
- [Big O Cheat Sheet](https://www.bigocheatsheet.com/)
- [Visualización de Algoritmos](https://visualgo.net/)
- [JavaScript Algorithms Repository](https://github.com/trekhleb/javascript-algorithms)
- [Introduction to Algorithms](https://mitpress.mit.edu/books/introduction-algorithms-fourth-edition)

¡Felicitaciones! 🎉 Has completado el tutorial de algoritmos y complejidad. Ahora tienes una comprensión básica de cómo analizar y optimizar algoritmos.
