# TypeScript en 30 Minutos - Guía Rápida

Aprenderás los conceptos fundamentales de TypeScript en solo 30 minutos! 🚀

## Requisitos Previos
- Node.js instalado
- Un editor de código (VS Code recomendado)
- Conocimientos básicos de JavaScript

## 1. Configuración Inicial (3 minutos)

```bash
# Instalar TypeScript globalmente
npm install -g typescript

# Crear proyecto
mkdir mi-proyecto-ts
cd mi-proyecto-ts
npm init -y

# Instalar TypeScript en el proyecto
npm install typescript --save-dev

# Inicializar configuración de TypeScript
npx tsc --init
```

> **Nota:** El archivo `tsconfig.json` generado contiene la configuración de TypeScript. Las opciones más importantes son:
> - `target`: Versión de JavaScript a la que se compilará
> - `module`: Sistema de módulos a utilizar
> - `strict`: Habilita todas las comprobaciones estrictas de tipos

## 2. Tipos Básicos (7 minutos)

```typescript
// tipos-basicos.ts

// Tipos primitivos
let nombre: string = "Juan";
let edad: number = 25;
let activo: boolean = true;
let indefinido: undefined = undefined;
let nulo: null = null;

// Arrays
let numeros: number[] = [1, 2, 3];
let nombres: Array<string> = ["Ana", "Juan"];

// Tuplas
let coordenada: [number, number] = [10, 20];
let usuarioData: [string, number] = ["admin", 1];

// Enum
enum DiaSemana {
    Lunes,
    Martes,
    Miercoles,
    Jueves,
    Viernes
}
let dia: DiaSemana = DiaSemana.Lunes;

// Any y Unknown
let cualquierCosa: any = 4;      // Evitar usar
let desconocido: unknown = 4;    // Más seguro que any
```

> **Nota sobre tipos:**
> - `string`, `number`, `boolean`: Tipos primitivos básicos
> - `Array<T>` o `T[]`: Arrays de un tipo específico
> - `tuple`: Array con número fijo de elementos de tipos específicos
> - `enum`: Conjunto de constantes nombradas
> - `any`: Desactiva la verificación de tipos (evitar usar)
> - `unknown`: Similar a any pero más seguro, requiere verificación de tipo

## 3. Interfaces y Tipos (7 minutos)

```typescript
// interfaces.ts

// Interface básica
interface Usuario {
    nombre: string;
    edad: number;
    email?: string;  // Propiedad opcional
    readonly id: number;  // Solo lectura
}

// Implementación
const usuario: Usuario = {
    nombre: "María",
    edad: 30,
    id: 1
};

// Type alias
type Punto = {
    x: number;
    y: number;
};

// Union types
type EstadoTarea = "pendiente" | "completada" | "cancelada";
let estado: EstadoTarea = "pendiente";

// Intersection types
interface Empleado {
    id: number;
    nombre: string;
}

interface Contacto {
    email: string;
    telefono: string;
}

type EmpleadoContacto = Empleado & Contacto;
```

> **Nota sobre interfaces y tipos:**
> - `interface`: Define la estructura que debe tener un objeto
> - `?`: Marca una propiedad como opcional
> - `readonly`: La propiedad solo puede ser asignada una vez
> - `type`: Similar a interface pero más flexible
> - `union types`: Permite que un valor sea de varios tipos
> - `intersection types`: Combina múltiples tipos en uno

## 4. Funciones (7 minutos)

```typescript
// funciones.ts

// Función con tipos
function sumar(a: number, b: number): number {
    return a + b;
}

// Función con parámetro opcional
function saludar(nombre: string, titulo?: string): string {
    return titulo ? `${titulo} ${nombre}` : `Hola ${nombre}`;
}

// Función con parámetro por defecto
function crearUsuario(
    nombre: string, 
    edad: number = 18
): Usuario {
    return { nombre, edad, id: Date.now() };
}

// Arrow function con tipos
const multiplicar = (a: number, b: number): number => a * b;

// Funciones como tipos
type Operacion = (a: number, b: number) => number;
const dividir: Operacion = (a, b) => a / b;

// Sobrecarga de funciones
function procesar(x: number): number;
function procesar(x: string): string;
function procesar(x: any): any {
    return typeof x === "number" ? x * 2 : x.toUpperCase();
}
```

> **Nota sobre funciones:**
> - Los parámetros y el valor de retorno pueden ser tipados
> - `?` marca un parámetro como opcional
> - Los parámetros opcionales deben ir al final
> - Se pueden definir valores por defecto
> - La sobrecarga permite diferentes implementaciones según los tipos

## 5. Clases (6 minutos)

```typescript
// clases.ts

class Persona {
    // Propiedades privadas y públicas
    private id: number;
    public nombre: string;
    protected edad: number;

    constructor(nombre: string, edad: number) {
        this.id = Date.now();
        this.nombre = nombre;
        this.edad = edad;
    }

    // Método público
    presentarse(): string {
        return `Hola, soy ${this.nombre}`;
    }

    // Getter
    get obtenerEdad(): number {
        return this.edad;
    }

    // Setter
    set establecerEdad(edad: number) {
        if (edad >= 0) {
            this.edad = edad;
        }
    }
}

// Herencia
class Empleado extends Persona {
    constructor(
        nombre: string,
        edad: number,
        private puesto: string
    ) {
        super(nombre, edad);
    }

    // Sobrescribir método
    presentarse(): string {
        return `${super.presentarse()}, trabajo como ${this.puesto}`;
    }
}
```

> **Nota sobre clases:**
> - `private`: Solo accesible dentro de la clase
> - `protected`: Accesible en la clase y sus subclases
> - `public`: Accesible desde cualquier lugar (por defecto)
> - Los getters y setters permiten controlar el acceso a propiedades
> - La herencia permite extender funcionalidad de otras clases

## Recursos Adicionales
- [Documentación oficial de TypeScript](https://www.typescriptlang.org/docs/)
- [TypeScript Playground](https://www.typescriptlang.org/play)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)

¡Felicitaciones! 🎉 Has completado el tutorial rápido de TypeScript. Estos son los fundamentos básicos para comenzar a desarrollar con tipos estáticos en JavaScript.
