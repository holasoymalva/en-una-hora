# Fundamentos de C++ - Guía de 1 Hora

Aprenderás los conceptos fundamentales de C++ de manera práctica y sencilla! 🚀

## Requisitos Previos
- Un compilador de C++ (g++ o MinGW)
- Un editor de código (VS Code recomendado)
- Conocimientos básicos de programación

## 1. Configuración y Primer Programa (5 minutos)

```cpp
// primer_programa.cpp
#include <iostream>
using namespace std;

int main() {
    cout << "¡Hola, mundo!" << endl;
    return 0;
}
```

Compilar y ejecutar:
```bash
# Linux/macOS
g++ -o programa primer_programa.cpp
./programa

# Windows
g++ -o programa.exe primer_programa.cpp
programa.exe
```

> **Nota sobre la estructura básica:**
> - `#include`: Incluye bibliotecas
> - `using namespace std`: Evita escribir std:: antes de cout, cin, etc.
> - `int main()`: Punto de entrada del programa
> - `return 0`: Indica que el programa terminó correctamente

## 2. Variables y Tipos de Datos (10 minutos)

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Tipos numéricos
    int edad = 25;              // Entero
    double precio = 19.99;      // Punto flotante
    float temperatura = 36.5f;  // Punto flotante (precisión simple)
    bool activo = true;         // Booleano
    char letra = 'A';           // Carácter
    
    // Strings (cadenas)
    string nombre = "Juan";     // Cadena de texto
    
    // Constantes
    const int MAX_VALOR = 100;
    
    // Arrays
    int numeros[5] = {1, 2, 3, 4, 5};
    
    // Mostrar valores
    cout << "Nombre: " << nombre << endl;
    cout << "Edad: " << edad << endl;
    cout << "Precio: $" << precio << endl;
    
    // Entrada de usuario
    string entrada;
    cout << "Escribe tu nombre: ";
    getline(cin, entrada);
    cout << "Hola, " << entrada << endl;
    
    return 0;
}
```

> **Nota sobre tipos de datos:**
> - Los tipos de datos tienen diferentes tamaños y rangos
> - `int`: Generalmente 4 bytes (-2,147,483,648 a 2,147,483,647)
> - `double`: 8 bytes para números decimales
> - `string`: Requiere `#include <string>`
> - Los arrays tienen tamaño fijo definido en la declaración

## 3. Control de Flujo (10 minutos)

```cpp
#include <iostream>
using namespace std;

int main() {
    // If-else
    int numero = 10;
    if (numero > 0) {
        cout << "Positivo" << endl;
    } else if (numero < 0) {
        cout << "Negativo" << endl;
    } else {
        cout << "Cero" << endl;
    }
    
    // Switch
    char opcion = 'A';
    switch (opcion) {
        case 'A':
            cout << "Opción A seleccionada" << endl;
            break;
        case 'B':
            cout << "Opción B seleccionada" << endl;
            break;
        default:
            cout << "Opción no válida" << endl;
    }
    
    // While
    int contador = 0;
    while (contador < 5) {
        cout << contador << " ";
        contador++;
    }
    cout << endl;
    
    // Do-while
    int i = 0;
    do {
        cout << "Ejecutando..." << endl;
        i++;
    } while (i < 3);
    
    // For
    for (int j = 0; j < 5; j++) {
        cout << j << " ";
    }
    cout << endl;
    
    // For con array
    int numeros[] = {1, 2, 3, 4, 5};
    for (int numero : numeros) {  // Range-based for loop (C++11)
        cout << numero << " ";
    }
    cout << endl;
    
    return 0;
}
```

> **Nota sobre control de flujo:**
> - Los bucles pueden interrumpirse con `break`
> - `continue` salta a la siguiente iteración
> - El range-based for loop requiere C++11 o superior
> - Siempre incluir `break` en casos de switch

## 4. Funciones (10 minutos)

```cpp
#include <iostream>
using namespace std;

// Declaración de funciones
int sumar(int a, int b);
void saludar(string nombre);
double calcularPromedio(int numeros[], int longitud);
void modificarValor(int& numero);  // Referencia

int main() {
    // Llamadas a funciones
    cout << "Suma: " << sumar(5, 3) << endl;
    saludar("Ana");
    
    int nums[] = {1, 2, 3, 4, 5};
    cout << "Promedio: " << calcularPromedio(nums, 5) << endl;
    
    int x = 10;
    cout << "Antes: " << x << endl;
    modificarValor(x);
    cout << "Después: " << x << endl;
    
    return 0;
}

// Definición de funciones
int sumar(int a, int b) {
    return a + b;
}

void saludar(string nombre) {
    cout << "¡Hola, " << nombre << "!" << endl;
}

double calcularPromedio(int numeros[], int longitud) {
    int suma = 0;
    for (int i = 0; i < longitud; i++) {
        suma += numeros[i];
    }
    return static_cast<double>(suma) / longitud;
}

void modificarValor(int& numero) {
    numero *= 2;  // Modifica el valor original
}
```

> **Nota sobre funciones:**
> - Las funciones deben declararse antes de usarse
> - `void` indica que la función no retorna valor
> - Los parámetros por referencia (`&`) modifican el original
> - `static_cast<tipo>` realiza conversiones explícitas

## 5. Clases y Objetos (15 minutos)

```cpp
#include <iostream>
#include <string>
using namespace std;

class Persona {
private:
    string nombre;
    int edad;
    
public:
    // Constructor
    Persona(string n, int e) {
        nombre = n;
        edad = e;
    }
    
    // Constructor por defecto
    Persona() {
        nombre = "Sin nombre";
        edad = 0;
    }
    
    // Métodos
    void saludar() {
        cout << "Hola, soy " << nombre << endl;
    }
    
    // Getters y setters
    string getNombre() {
        return nombre;
    }
    
    void setNombre(string n) {
        nombre = n;
    }
    
    int getEdad() {
        return edad;
    }
    
    void setEdad(int e) {
        if (e >= 0) {
            edad = e;
        }
    }
    
    // Método estático
    static void mensaje() {
        cout << "Soy un método estático" << endl;
    }
};

// Herencia
class Estudiante : public Persona {
private:
    string carrera;
    
public:
    Estudiante(string n, int e, string c) : Persona(n, e) {
        carrera = c;
    }
    
    void estudiar() {
        cout << getNombre() << " está estudiando " << carrera << endl;
    }
};

int main() {
    // Crear objetos
    Persona persona1("Juan", 25);
    persona1.saludar();
    
    Estudiante estudiante1("María", 20, "Informática");
    estudiante1.saludar();
    estudiante1.estudiar();
    
    // Método estático
    Persona::mensaje();
    
    return 0;
}
```

> **Nota sobre POO:**
> - Las clases encapsulan datos y comportamiento
> - `private` y `public` controlan el acceso
> - Los constructores inicializan objetos
> - La herencia permite reutilizar código
> - Los métodos estáticos pertenecen a la clase

## 6. Vectores y STL (10 minutos)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // Vector básico
    vector<int> numeros;
    numeros.push_back(1);
    numeros.push_back(2);
    numeros.push_back(3);
    
    // Vector con tamaño inicial
    vector<string> nombres(3, "N/A");
    
    // Acceder a elementos
    cout << "Primer número: " << numeros[0] << endl;
    cout << "Último número: " << numeros.back() << endl;
    
    // Iterar sobre vector
    for (int num : numeros) {
        cout << num << " ";
    }
    cout << endl;
    
    // Algoritmos STL
    // Ordenar
    sort(numeros.begin(), numeros.end());
    
    // Encontrar elemento
    auto it = find(numeros.begin(), numeros.end(), 2);
    if (it != numeros.end()) {
        cout << "Encontrado: " << *it << endl;
    }
    
    // Tamaño y capacidad
    cout << "Tamaño: " << numeros.size() << endl;
    cout << "Capacidad: " << numeros.capacity() << endl;
    
    // Eliminar elementos
    numeros.pop_back();  // Elimina el último
    numeros.clear();     // Elimina todos
    
    return 0;
}
```

> **Nota sobre STL:**
> - Vector es un array dinámico
> - STL proporciona contenedores y algoritmos
> - `auto` infiere el tipo automáticamente
> - Los iteradores permiten recorrer contenedores
> - Muchos algoritmos requieren contenedores ordenados

## Recursos Adicionales
- [CPlusPlus.com](http://www.cplusplus.com/)
- [C++ Reference](https://en.cppreference.com/)
- [LearnCpp.com](https://www.learncpp.com/)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

¡Felicitaciones! 🎉 Has completado el tutorial de fundamentos de C++. Ahora tienes las bases necesarias para comenzar a programar en este lenguaje.
