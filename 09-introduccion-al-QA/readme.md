# Introducción al QA Engineering - Guía de 1 Hora

Aprenderás los conceptos fundamentales del Quality Assurance (QA) y cómo implementar pruebas automatizadas. 🧪

## Requisitos Previos
- Conocimientos básicos de programación
- Un editor de código (VS Code recomendado)
- Node.js instalado
- Nociones básicas de JavaScript

## 1. Fundamentos de QA (10 minutos)

### Tipos de Pruebas
1. **Pruebas Unitarias**
   - Prueban unidades individuales de código
   - Rápidas y aisladas
   - Base de la pirámide de pruebas

2. **Pruebas de Integración**
   - Prueban interacciones entre componentes
   - Verifican flujos completos
   - Nivel medio de la pirámide

3. **Pruebas E2E (End-to-End)**
   - Prueban el sistema completo
   - Simulan comportamiento de usuario
   - Cima de la pirámide

> **Nota sobre Tipos de Pruebas:**
> - Las unitarias son la base: muchas y rápidas
> - Las de integración: cantidad media
> - Las E2E: pocas pero importantes
> - Mantener balance entre tipos

## 2. Pruebas Unitarias con Jest (15 minutos)

```javascript
// src/calculator.js
class Calculator {
    add(a, b) {
        return a + b;
    }

    subtract(a, b) {
        return a - b;
    }

    multiply(a, b) {
        return a * b;
    }

    divide(a, b) {
        if (b === 0) {
            throw new Error('División por cero no permitida');
        }
        return a / b;
    }
}

module.exports = Calculator;

// tests/calculator.test.js
const Calculator = require('../src/calculator');

describe('Calculator', () => {
    let calculator;

    beforeEach(() => {
        calculator = new Calculator();
    });

    describe('add', () => {
        test('suma dos números positivos correctamente', () => {
            expect(calculator.add(2, 3)).toBe(5);
        });

        test('suma números negativos correctamente', () => {
            expect(calculator.add(-2, -3)).toBe(-5);
        });
    });

    describe('divide', () => {
        test('lanza error al dividir por cero', () => {
            expect(() => calculator.divide(5, 0)).toThrow('División por cero');
        });
    });
});
```

> **Nota sobre Jest:**
> - `describe` agrupa pruebas relacionadas
> - `test` o `it` define un caso de prueba
> - `expect` realiza aserciones
> - `beforeEach` prepara el entorno de prueba

## 3. Pruebas de API con Supertest (15 minutos)

```javascript
// src/app.js
const express = require('express');
const app = express();

app.use(express.json());

const users = [];

app.get('/users', (req, res) => {
    res.json(users);
});

app.post('/users', (req, res) => {
    const { name, email } = req.body;
    if (!name || !email) {
        return res.status(400).json({ error: 'Name and email required' });
    }
    
    const user = { id: users.length + 1, name, email };
    users.push(user);
    res.status(201).json(user);
});

module.exports = app;

// tests/api.test.js
const request = require('supertest');
const app = require('../src/app');

describe('API Tests', () => {
    beforeEach(() => {
        // Limpiar datos de prueba
        users.length = 0;
    });

    describe('GET /users', () => {
        test('retorna lista vacía inicialmente', async () => {
            const response = await request(app)
                .get('/users')
                .expect(200);
            
            expect(response.body).toEqual([]);
        });
    });

    describe('POST /users', () => {
        test('crea un nuevo usuario', async () => {
            const userData = {
                name: 'John Doe',
                email: 'john@example.com'
            };

            const response = await request(app)
                .post('/users')
                .send(userData)
                .expect(201);

            expect(response.body).toMatchObject(userData);
            expect(response.body.id).toBeDefined();
        });

        test('retorna error si faltan datos', async () => {
            const response = await request(app)
                .post('/users')
                .send({})
                .expect(400);

            expect(response.body.error).toBeDefined();
        });
    });
});
```

## 4. Pruebas E2E con Cypress (10 minutos)

```javascript
// cypress/integration/login.spec.js
describe('Login Page', () => {
    beforeEach(() => {
        cy.visit('/login');
    });

    it('muestra mensaje de error con credenciales inválidas', () => {
        cy.get('input[name="email"]')
            .type('usuario@ejemplo.com');
        
        cy.get('input[name="password"]')
            .type('contraseña123');
        
        cy.get('button[type="submit"]')
            .click();
        
        cy.get('.error-message')
            .should('be.visible')
            .and('contain', 'Credenciales inválidas');
    });

    it('redirige al dashboard con login exitoso', () => {
        cy.get('input[name="email"]')
            .type('admin@ejemplo.com');
        
        cy.get('input[name="password"]')
            .type('admin123');
        
        cy.get('button[type="submit"]')
            .click();
        
        cy.url().should('include', '/dashboard');
    });
});
```

> **Nota sobre Cypress:**
> - Sintaxis legible y fluida
> - Esperas automáticas
> - Debugging visual
> - Grabación de pruebas

## 5. Ejemplo Práctico: Sistema de Login (10 minutos)

```javascript
// src/login.js
class LoginSystem {
    constructor() {
        this.users = [
            { email: 'admin@example.com', password: 'admin123' }
        ];
    }

    validateEmail(email) {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(email);
    }

    login(email, password) {
        if (!this.validateEmail(email)) {
            throw new Error('Email inválido');
        }

        const user = this.users.find(u => 
            u.email === email && u.password === password
        );

        if (!user) {
            throw new Error('Credenciales inválidas');
        }

        return { success: true, message: 'Login exitoso' };
    }
}

// tests/login.test.js
describe('LoginSystem', () => {
    let loginSystem;

    beforeEach(() => {
        loginSystem = new LoginSystem();
    });

    describe('validateEmail', () => {
        test('acepta email válido', () => {
            expect(loginSystem.validateEmail('user@example.com')).toBe(true);
        });

        test('rechaza email inválido', () => {
            expect(loginSystem.validateEmail('invalido')).toBe(false);
        });
    });

    describe('login', () => {
        test('permite login con credenciales correctas', () => {
            const result = loginSystem.login('admin@example.com', 'admin123');
            expect(result.success).toBe(true);
        });

        test('rechaza email inválido', () => {
            expect(() => 
                loginSystem.login('invalido', 'pass')
            ).toThrow('Email inválido');
        });

        test('rechaza credenciales incorrectas', () => {
            expect(() => 
                loginSystem.login('admin@example.com', 'wrong')
            ).toThrow('Credenciales inválidas');
        });
    });
});
```

## Mejores Prácticas

1. **Organización de Pruebas**
   - Una prueba, un concepto
   - Nombres descriptivos
   - Estructura AAA (Arrange-Act-Assert)
   - Usar fixtures para datos de prueba

2. **Mantenibilidad**
   - DRY en pruebas
   - Evitar pruebas frágiles
   - Documentar casos especiales
   - Usar helpers comunes

3. **Cobertura**
   - Definir objetivos de cobertura
   - Priorizar casos críticos
   - Balancear tipos de pruebas
   - Mantener pruebas actualizadas

4. **Automatización**
   - Integrar con CI/CD
   - Ejecutar pruebas en paralelo
   - Reportes automáticos
   - Monitoreo de tiempo de ejecución

## Herramientas Recomendadas

1. **Frameworks de Pruebas**
   - Jest
   - Mocha
   - Jasmine
   - Cypress

2. **Aserciones**
   - Chai
   - Jest Matchers
   - Should.js

3. **Mocking**
   - Sinon
   - Jest Mocks
   - Nock

4. **Reportes**
   - Jest Coverage
   - Mochawesome
   - Allure

## Recursos Adicionales
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [Cypress Documentation](https://docs.cypress.io/)
- [Testing JavaScript](https://testingjavascript.com/)
- [QA Interview Questions](https://github.com/testing-interview/qa-interview-questions)

¡Felicitaciones! 🎉 Has completado la introducción al QA Engineering. Recuerda que la calidad del software es responsabilidad de todo el equipo.
