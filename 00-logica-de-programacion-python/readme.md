
# Introducción a la Lógica de Programación con Python

Aprenderás los fundamentos de la lógica de programación utilizando Python como herramienta. 🧠

## Requisitos Previos
- Python 3.x instalado
- Un editor de código (IDLE o VS Code)
- No se requiere experiencia previa en programación

## 1. Pensamiento Secuencial (10 minutos)

### Ejemplo: Calcular el promedio de tres números

```python
# 1. Entrada de datos
nota1 = float(input("Ingresa la primera nota: "))
nota2 = float(input("Ingresa la segunda nota: "))
nota3 = float(input("Ingresa la tercera nota: "))

# 2. Proceso (cálculo)
promedio = (nota1 + nota2 + nota3) / 3

# 3. Salida de resultados
print(f"El promedio es: {promedio:.2f}")
```

> **Nota sobre Pensamiento Secuencial:**
> - Los programas se ejecutan línea por línea
> - Cada instrucción tiene un propósito específico
> - Identificar claramente: Entrada → Proceso → Salida
> - La indentación en Python es importante

## 2. Variables y Tipos de Datos (10 minutos)

```python
# Números
edad = 25                # Entero (int)
altura = 1.75           # Decimal (float)
temperatura = -5        # Números negativos

# Texto
nombre = "Ana"          # String (str)
apellido = 'García'     # Comillas simples o dobles

# Booleanos
es_estudiante = True    # Boolean (bool)
tiene_mascota = False

# Mostrar tipos de datos
print(type(edad))       # <class 'int'>
print(type(nombre))     # <class 'str'>
print(type(altura))     # <class 'float'>

# Conversión de tipos
edad_texto = str(edad)            # Número a texto
numero = int("100")              # Texto a número
decimal = float("3.14")         # Texto a decimal
```

> **Nota sobre Variables:**
> - Las variables son como cajas que almacenan datos
> - El nombre debe ser descriptivo
> - Python es sensible a mayúsculas/minúsculas
> - Los tipos de datos determinan qué operaciones son posibles

## 3. Operadores y Expresiones (10 minutos)

```python
# Operadores aritméticos
suma = 5 + 3           # 8
resta = 10 - 4         # 6
multiplicacion = 3 * 4  # 12
division = 15 / 3      # 5.0
division_entera = 17 // 3  # 5
modulo = 17 % 5        # 2
potencia = 2 ** 3      # 8

# Operadores de comparación
es_mayor = 5 > 3       # True
es_menor = 5 < 3       # False
es_igual = 5 == 5      # True
es_diferente = 5 != 3  # True
mayor_igual = 5 >= 5   # True
menor_igual = 5 <= 6   # True

# Operadores lógicos
tiene_edad = edad >= 18
tiene_permiso = True
puede_conducir = tiene_edad and tiene_permiso

# Expresiones combinadas
resultado = (5 + 3) * 2  # Los paréntesis definen prioridad
descuento = precio * 0.1 if precio > 100 else 0
```

> **Nota sobre Operadores:**
> - Los operadores aritméticos realizan cálculos
> - Los operadores de comparación devuelven True/False
> - Los operadores lógicos combinan condiciones
> - Los paréntesis controlan el orden de evaluación

## 4. Estructuras de Control (15 minutos)

### Condicionales
```python
# If simple
edad = int(input("Ingresa tu edad: "))

if edad >= 18:
    print("Eres mayor de edad")
    print("Puedes votar")
else:
    print("Eres menor de edad")

# If con múltiples condiciones
nota = float(input("Ingresa tu nota: "))

if nota >= 90:
    print("Excelente")
elif nota >= 80:
    print("Muy bien")
elif nota >= 70:
    print("Bien")
else:
    print("Necesitas mejorar")

# Condicionales anidados
tiene_pasaporte = True
tiene_visa = True

if tiene_pasaporte:
    if tiene_visa:
        print("Puedes viajar")
    else:
        print("Necesitas visa")
else:
    print("Necesitas pasaporte")
```

### Bucles
```python
# While (mientras)
contador = 1
while contador <= 5:
    print(f"Contador: {contador}")
    contador += 1

# For (para)
for i in range(5):  # 0 a 4
    print(f"Número: {i}")

# For con lista
frutas = ["manzana", "banana", "naranja"]
for fruta in frutas:
    print(f"Me gusta la {fruta}")

# Break y Continue
for numero in range(10):
    if numero == 5:
        break  # Sale del bucle
    if numero % 2 == 0:
        continue  # Salta a la siguiente iteración
    print(numero)
```

> **Nota sobre Estructuras de Control:**
> - If/elif/else para decisiones
> - While para bucles con condición
> - For para iteraciones definidas
> - Break sale del bucle
> - Continue salta a la siguiente iteración

## 5. Ejercicio Práctico: Juego de Adivinanza (15 minutos)

```python
import random

def juego_adivinanza():
    # Generar número aleatorio entre 1 y 100
    numero_secreto = random.randint(1, 100)
    intentos = 0
    max_intentos = 7

    print("¡Bienvenido al juego de adivinanza!")
    print(f"Tienes {max_intentos} intentos para adivinar un número entre 1 y 100")

    while intentos < max_intentos:
        # Entrada del usuario
        try:
            intento = int(input("\nIngresa tu número: "))
        except ValueError:
            print("Por favor, ingresa un número válido")
            continue

        intentos += 1

        # Verificar el intento
        if intento < 1 or intento > 100:
            print("El número debe estar entre 1 y 100")
            continue

        # Dar pistas
        if intento == numero_secreto:
            print(f"¡Felicitaciones! Adivinaste en {intentos} intentos")
            return
        elif intento < numero_secreto:
            print("El número es mayor")
        else:
            print("El número es menor")
        
        # Mostrar intentos restantes
        print(f"Te quedan {max_intentos - intentos} intentos")

    print(f"\nGame Over. El número era {numero_secreto}")

# Ejecutar el juego
juego_adivinanza()
```

> **Nota sobre el Ejercicio:**
> - Combina varios conceptos: variables, bucles, condicionales
> - Maneja entrada del usuario
> - Implementa lógica de juego
> - Usa control de errores (try/except)

## Conceptos Clave de Lógica de Programación

1. **Descomposición**
   - Dividir problemas grandes en problemas más pequeños
   - Resolver cada parte por separado
   - Combinar las soluciones

2. **Reconocimiento de Patrones**
   - Identificar similitudes entre problemas
   - Reutilizar soluciones conocidas
   - Adaptar soluciones existentes

3. **Abstracción**
   - Enfocarse en los detalles importantes
   - Ignorar información irrelevante
   - Simplificar problemas complejos

4. **Pensamiento Algorítmico**
   - Desarrollar pasos ordenados
   - Crear soluciones sistemáticas
   - Optimizar procesos

## Ejercicios Adicionales

1. Calculadora Simple
   - Crear una calculadora que realice operaciones básicas
   - Usar condicionales para seleccionar la operación
   - Manejar errores de entrada

2. Conversor de Temperatura
   - Convertir entre Celsius y Fahrenheit
   - Usar funciones para organizar el código
   - Implementar un menú de opciones

3. Lista de Tareas
   - Crear una lista de tareas simple
   - Permitir agregar y eliminar tareas
   - Mostrar las tareas pendientes

## Recursos Adicionales
- [Python para Principiantes](https://www.python.org/about/gettingstarted/)
- [Codecademy Python](https://www.codecademy.com/learn/learn-python-3)
- [Python Tutor](http://pythontutor.com/)
- [Ejercicios de Práctica](https://www.practicepython.org/)

¡Felicitaciones! 🎉 Has completado la introducción a la lógica de programación con Python. Recuerda que la práctica constante es la clave para mejorar tus habilidades de programación.
