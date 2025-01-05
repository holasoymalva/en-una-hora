# Clean Code y Buenas Prácticas en JavaScript - Guía de 1 Hora

Aprenderás los principios fundamentales del código limpio y las mejores prácticas al programar en JavaScript. 🧹

## 1. Nombres Significativos (10 minutos)

### Malas Prácticas ❌
```javascript
// Variables con nombres poco descriptivos
let d; // días
let temp; // temporal
let arr; // array de usuarios

// Funciones con nombres ambiguos
function process() {}
function handle() {}
function manage() {}
```

### Buenas Prácticas ✅
```javascript
// Variables con nombres descriptivos
const maxDaysInMonth = 31;
const activeUsers = ['Juan', 'Ana'];
const userConfiguration = {
    theme: 'dark',
    language: 'es'
};

// Funciones con nombres que indican acción
function calculateTotalPrice(products) {}
function validateUserCredentials(username, password) {}
function sendWelcomeEmail(userEmail) {}

// Usar sustantivos para clases/objetos
class UserAccount {}
class ProductInventory {}

// Usar verbos para funciones/métodos
function getUserData() {}
function updateUserProfile() {}
```

> **Notas sobre Nombres:**
> - Usar nombres descriptivos que expliquen el propósito
> - Evitar abreviaciones poco claras
> - Los nombres deben ser pronunciables y buscables
> - Usar convenciones consistentes (camelCase para JavaScript)
> - Nombres de clases: sustantivos, primera letra mayúscula
> - Nombres de funciones: verbos o frases verbales
> - Ser específico y evitar términos genéricos

## 2. Funciones (10 minutos)

### Malas Prácticas ❌
```javascript
// Función que hace demasiadas cosas
function processUser(user) {
    validateUser(user);
    updateDatabase(user);
    sendEmail(user);
    updateLogs(user);
    notifyAdmin(user);
}

// Muchos parámetros
function createUser(name, email, age, city, country, phone, role) {}

// Mezcla de niveles de abstracción
function sendEmail(user) {
    const smtp = new SMTPClient();
    smtp.connect();
    validateEmail(user.email);
    formatEmailContent();
    smtp.send();
    logEmailSent();
}
```

### Buenas Prácticas ✅
```javascript
// Funciones pequeñas y con un solo propósito
function processUser(user) {
    if (!isUserValid(user)) {
        throw new Error('Invalid user');
    }
    
    saveUser(user);
    notifyUserCreation(user);
}

// Usar objetos para múltiples parámetros
function createUser(userDetails) {
    const {
        name,
        email,
        age,
        address,
        role
    } = userDetails;
    
    // Lógica de creación
}

// Mantener mismo nivel de abstracción
function sendEmail(user) {
    validateEmailAddress(user.email);
    const emailContent = prepareEmailContent(user);
    return emailService.send(user.email, emailContent);
}

// Evitar efectos secundarios
function calculateTotal(items) {
    return items.reduce((total, item) => total + item.price, 0);
}
```

> **Notas sobre Funciones:**
> - Mantener funciones pequeñas (máximo 20 líneas)
> - Una función debe hacer una sola cosa
> - Evitar efectos secundarios inesperados
> - Usar parámetros con nombres descriptivos
> - Limitar el número de parámetros (máximo 3)
> - Mantener consistente el nivel de abstracción

## 3. Comentarios y Documentación (10 minutos)

### Malas Prácticas ❌
```javascript
// Comentarios obvios
// Incrementar contador
counter++;

// Comentarios desactualizados
// Verificar si el usuario está activo
function checkStatus() {  // Ahora también verifica el rol
    // ...
}

// Código comentado
function processData() {
    // const oldWay = getData();
    // if (oldWay) { ... }
    const newWay = getDataV2();
}
```

### Buenas Prácticas ✅
```javascript
/**
 * Calcula el total de la factura incluyendo impuestos y descuentos.
 * @param {Object[]} items - Array de productos
 * @param {number} taxRate - Porcentaje de impuestos (0-100)
 * @param {number} discount - Monto del descuento
 * @returns {number} Total calculado
 */
function calculateInvoiceTotal(items, taxRate, discount) {
    const subtotal = calculateSubtotal(items);
    const tax = calculateTax(subtotal, taxRate);
    return subtotal + tax - discount;
}

// Comentarios para explicar el "por qué" no el "qué"
function validatePassword(password) {
    // Requerimiento de seguridad: mínimo 8 caracteres con números y símbolos
    const passwordRegex = /^(?=.*\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{8,}$/;
    return passwordRegex.test(password);
}
```

> **Notas sobre Comentarios:**
> - El mejor comentario es un buen nombre de variable o función
> - Comentar solo cuando el código no puede ser más claro
> - Mantener los comentarios actualizados
> - Usar JSDoc para documentar APIs y funciones públicas
> - Explicar el "por qué" no el "qué"
> - Evitar código comentado

## 4. Manejo de Errores (10 minutos)

### Malas Prácticas ❌
```javascript
// Ignorar errores
try {
    riskyOperation();
} catch (e) {}

// Retornar null
function findUser(id) {
    const user = database.find(id);
    if (!user) return null;
    return user;
}

// Mensajes de error genéricos
throw new Error('Error!');
```

### Buenas Prácticas ✅
```javascript
// Crear clases de error personalizadas
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = 'ValidationError';
    }
}

// Manejar errores apropiadamente
async function getUserData(userId) {
    try {
        const user = await database.findUser(userId);
        
        if (!user) {
            throw new ValidationError(`User with id ${userId} not found`);
        }
        
        return user;
    } catch (error) {
        if (error instanceof ValidationError) {
            // Manejar error de validación
            console.error('Validation failed:', error.message);
            return null;
        }
        
        // Registro y reenvío de errores inesperados
        console.error('Unexpected error:', error);
        throw error;
    }
}

// Usar objetos Result para operaciones que pueden fallar
function divide(a, b) {
    if (b === 0) {
        return {
            success: false,
            error: 'Division by zero is not allowed'
        };
    }
    
    return {
        success: true,
        data: a / b
    };
}

// Uso
const result = divide(10, 2);
if (result.success) {
    console.log('Result:', result.data);
} else {
    console.error('Error:', result.error);
}
```

> **Notas sobre Manejo de Errores:**
> - Siempre manejar los errores
> - Usar tipos de error específicos
> - Proporcionar mensajes de error útiles
> - Considerar el uso de objetos Result
> - Mantener la consistencia en el manejo de errores
> - Registrar errores apropiadamente

## 5. Organización del Código (10 minutos)

### Estructura de Proyecto
```
src/
├── config/
│   ├── database.js
│   └── app.js
├── models/
│   ├── user.js
│   └── product.js
├── services/
│   ├── authService.js
│   └── emailService.js
├── utils/
│   ├── validation.js
│   └── formatting.js
├── controllers/
│   ├── userController.js
│   └── productController.js
└── index.js
```

### Módulos y Dependencias
```javascript
// Malas Prácticas ❌
// Archivo demasiado grande con múltiples responsabilidades
// userManager.js - 500 líneas de código mezclando lógica

// Buenas Prácticas ✅
// auth.service.js
export class AuthService {
    async login(credentials) { /* ... */ }
    async logout() { /* ... */ }
}

// user.service.js
export class UserService {
    async createUser(userData) { /* ... */ }
    async updateUser(userId, updates) { /* ... */ }
}

// validation.utils.js
export const validateEmail = (email) => { /* ... */ };
export const validatePassword = (password) => { /* ... */ };
```

> **Notas sobre Organización:**
> - Separar código por responsabilidades
> - Usar una estructura de carpetas clara
> - Mantener los archivos pequeños y enfocados
> - Seguir el principio de responsabilidad única
> - Organizar imports de manera consistente
> - Usar paths aliases para imports más limpios

## 6. Ejercicio Práctico (10 minutos)

### Código Original (Con malas prácticas)
```javascript
// Procesador de pedidos
function p(d) {
    let t = 0;
    let e = [];
    
    for(let i = 0; i < d.length; i++) {
        if(d[i].q > 0) {
            try {
                let p = d[i].p * d[i].q;
                if(d[i].d) p = p * 0.9;
                t += p;
                e.push({i: d[i].i, q: d[i].q, p: p});
            } catch(e) {
                console.log('error');
            }
        }
    }
    
    return {t: t, e: e};
}
```

### Código Refactorizado (Clean Code)
```javascript
class OrderProcessor {
    static DISCOUNT_RATE = 0.9;

    /**
     * Procesa una lista de items y calcula el total del pedido.
     * @param {Array<OrderItem>} orderItems - Lista de items del pedido
     * @returns {OrderSummary} Resumen del pedido procesado
     */
    processOrder(orderItems) {
        try {
            const validItems = this.filterValidItems(orderItems);
            const processedItems = this.processItems(validItems);
            const total = this.calculateTotal(processedItems);

            return {
                total,
                items: processedItems
            };
        } catch (error) {
            throw new OrderProcessingError(
                `Failed to process order: ${error.message}`
            );
        }
    }

    filterValidItems(items) {
        return items.filter(item => item.quantity > 0);
    }

    processItems(items) {
        return items.map(item => ({
            id: item.id,
            quantity: item.quantity,
            price: this.calculateItemPrice(item)
        }));
    }

    calculateItemPrice(item) {
        const basePrice = item.price * item.quantity;
        return item.hasDiscount 
            ? basePrice * OrderProcessor.DISCOUNT_RATE 
            : basePrice;
    }

    calculateTotal(items) {
        return items.reduce((total, item) => total + item.price, 0);
    }
}

class OrderProcessingError extends Error {
    constructor(message) {
        super(message);
        this.name = 'OrderProcessingError';
    }
}

// Uso
const processor = new OrderProcessor();
const orderItems = [
    { id: 1, quantity: 2, price: 10, hasDiscount: true },
    { id: 2, quantity: 1, price: 20, hasDiscount: false }
];

try {
    const result = processor.processOrder(orderItems);
    console.log('Order processed:', result);
} catch (error) {
    console.error('Failed to process order:', error.message);
}
```

## Recursos Adicionales
- [Clean Code - Robert C. Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [ESLint - Herramienta de análisis de código](https://eslint.org/)
- [Prettier - Formateador de código](https://prettier.io/)
- [JavaScript Style Guide de Airbnb](https://github.com/airbnb/javascript)

¡Felicitaciones! 🎉 Has completado la guía de Clean Code y buenas prácticas en JavaScript. Ahora tienes las herramientas necesarias para escribir código más limpio y mantenible.
