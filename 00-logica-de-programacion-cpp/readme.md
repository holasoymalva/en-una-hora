# Lógica de Programación con C++ - Guía de 1 Hora

Aprenderás los conceptos fundamentales de la lógica de programación utilizando C++ como herramienta. 🧠

## Requisitos Previos
- Un compilador de C++ (g++ o MinGW)
- Un editor de código (VS Code recomendado)
- No se requiere experiencia previa en programación

## 1. Pensamiento Secuencial (10 minutos)

### Ejemplo: Calcular el área de un rectángulo
```cpp
#include <iostream>
using namespace std;

int main() {
    // 1. Declarar variables para almacenar datos
    float base, altura, area;
    
    // 2. Solicitar datos al usuario
    cout << "Ingrese la base del rectángulo: ";
    cin >> base;
    
    cout << "Ingrese la altura del rectángulo: ";
    cin >> altura;
    
    // 3. Realizar el cálculo
    area = base * altura;
    
    // 4. Mostrar el resultado
    cout << "El área del rectángulo es: " << area << endl;
    
    return 0;
}
```

> **Nota sobre Pensamiento Secuencial:**
> - Los programas se ejecutan línea por línea, de arriba a abajo
> - Cada instrucción debe tener un propósito claro
> - Es importante planificar los pasos antes de programar
> - Identificar las entradas, el proceso y las salidas

## 2. Estructuras de Decisión (15 minutos)

### Ejemplo: Determinar si un número es par o impar
```cpp
#include <iostream>
using namespace std;

int main() {
    // Problema: Determinar si un número es par o impar
    int numero;
    
    // Entrada
    cout << "Ingrese un número: ";
    cin >> numero;
    
    // Decisión simple
    if (numero % 2 == 0) {
        cout << numero << " es par" << endl;
    } else {
        cout << numero << " es impar" << endl;
    }
    
    // Decisión múltiple
    if (numero > 0) {
        cout << "El número es positivo" << endl;
    } else if (numero < 0) {
        cout << "El número es negativo" << endl;
    } else {
        cout << "El número es cero" << endl;
    }
    
    // Decisión anidada
    if (numero != 0) {
        if (numero > 0) {
            if (numero % 2 == 0) {
                cout << "Es un número par positivo" << endl;
            } else {
                cout << "Es un número impar positivo" << endl;
            }
        } else {
            cout << "Es un número negativo" << endl;
        }
    } else {
        cout << "El número es cero" << endl;
    }
    
    return 0;
}
```

> **Nota sobre Estructuras de Decisión:**
> - `if` evalúa una condición booleana
> - Las condiciones pueden usar operadores: ==, !=, >, <, >=, <=
> - Se pueden combinar condiciones con && (AND) y || (OR)
> - Las decisiones anidadas deben usarse con cuidado para mantener el código legible

## 3. Bucles y Repetición (15 minutos)

### Ejemplo: Calcular la suma de los primeros N números
```cpp
#include <iostream>
using namespace std;

int main() {
    int n, suma = 0;
    
    // Solicitar cantidad de números
    cout << "¿Cuántos números desea sumar? ";
    cin >> n;
    
    // Usando while
    cout << "\nUsando while:" << endl;
    int i = 1;
    while (i <= n) {
        suma += i;
        cout << "Suma parcial: " << suma << endl;
        i++;
    }
    cout << "La suma total es: " << suma << endl;
    
    // Usando for
    cout << "\nUsando for:" << endl;
    suma = 0;  // Reiniciar suma
    for (int j = 1; j <= n; j++) {
        suma += j;
        cout << "Suma parcial: " << suma << endl;
    }
    cout << "La suma total es: " << suma << endl;
    
    // Bucle con control de usuario
    cout << "\nEjemplo de do-while:" << endl;
    char continuar;
    do {
        cout << "¿Desea continuar? (s/n): ";
        cin >> continuar;
    } while (continuar == 's' || continuar == 'S');
    
    return 0;
}
```

> **Nota sobre Bucles:**
> - `while`: Se usa cuando no sabemos cuántas iteraciones necesitamos
> - `for`: Se usa cuando conocemos el número exacto de iteraciones
> - `do-while`: Garantiza al menos una ejecución
> - Los bucles pueden crear bucles infinitos si no se controlan bien

## 4. Resolución de Problemas (20 minutos)

### Ejemplo: Juego de Adivinanza
```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

int main() {
    // Inicializar generador de números aleatorios
    srand(time(0));
    
    // Generar número aleatorio entre 1 y 100
    int numeroSecreto = rand() % 100 + 1;
    int intento, intentos = 0;
    bool adivinado = false;
    
    cout << "¡Bienvenido al juego de adivinanza!" << endl;
    cout << "He pensado en un número entre 1 y 100." << endl;
    
    // Bucle principal del juego
    do {
        // 1. Obtener intento del usuario
        cout << "\nIngresa tu intento: ";
        cin >> intento;
        intentos++;
        
        // 2. Verificar el intento
        if (intento == numeroSecreto) {
            adivinado = true;
            cout << "¡Felicitaciones! Has adivinado en " << intentos << " intentos." << endl;
        } else {
            // 3. Dar pistas
            if (intento > numeroSecreto) {
                cout << "El número es menor." << endl;
            } else {
                cout << "El número es mayor." << endl;
            }
            
            // 4. Verificar si alcanzó el máximo de intentos
            if (intentos == 10) {
                cout << "¡Has agotado tus 10 intentos! El número era: " << numeroSecreto << endl;
                break;
            }
            
            cout << "Te quedan " << (10 - intentos) << " intentos." << endl;
        }
    } while (!adivinado);
    
    return 0;
}
```

> **Nota sobre Resolución de Problemas:**
> 1. Entender el problema claramente
> 2. Dividir el problema en pasos más pequeños
> 3. Identificar las estructuras de control necesarias
> 4. Implementar la solución paso a paso
> 5. Probar con diferentes casos

## Ejercicios Prácticos

1. Calculadora Simple
```cpp
// Crear una calculadora que realice las 4 operaciones básicas
// Usar switch para seleccionar la operación
// Manejar la división por cero
```

2. Tabla de Multiplicar
```cpp
// Generar la tabla de multiplicar de un número
// Usar un bucle for para mostrar los resultados
// Formatear la salida para que se vea ordenada
```

3. Validación de Contraseña
```cpp
// Pedir una contraseña al usuario
// Verificar que cumpla con ciertos criterios (longitud, números, etc.)
// Dar retroalimentación específica sobre los requisitos faltantes
```

## Tips para Desarrollar la Lógica de Programación

1. **Descomponer Problemas**
   - Dividir problemas grandes en problemas más pequeños
   - Resolver cada subproblema por separado
   - Integrar las soluciones

2. **Planificar antes de Codificar**
   - Escribir los pasos en pseudocódigo
   - Identificar entradas y salidas
   - Determinar las estructuras de control necesarias

3. **Depuración Efectiva**
   - Usar cout para verificar valores intermedios
   - Probar casos límite
   - Verificar paso a paso la ejecución

4. **Buenas Prácticas**
   - Usar nombres descriptivos para variables
   - Mantener el código ordenado y comentado
   - Probar el código con diferentes entradas

## Recursos Adicionales
- [Programación ATS (YouTube)](https://www.youtube.com/c/ProgramacionATS)
- [C++ Tutorials (W3Schools)](https://www.w3schools.com/cpp/)
- [Ejercicios de Programación](https://www.programiz.com/cpp-programming/examples)

¡Felicitaciones! 🎉 Has completado el tutorial de lógica de programación con C++. Recuerda que la práctica es fundamental para desarrollar un buen pensamiento lógico.
