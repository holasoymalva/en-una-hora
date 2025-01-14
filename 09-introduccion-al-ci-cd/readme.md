# Introducción al CI/CD - Guía de 1 Hora

Aprenderás los conceptos fundamentales de Integración Continua (CI) y Entrega/Despliegue Continuo (CD), con ejemplos prácticos. 🚀

## Requisitos Previos
- Conocimientos básicos de Git
- Cuenta en GitHub
- Nociones básicas de desarrollo web

## 1. Fundamentos de CI/CD

### ¿Qué es CI/CD?
- **CI (Integración Continua)**
  - Integrar código frecuentemente
  - Detectar problemas temprano
  - Automatizar pruebas

- **CD (Entrega/Despliegue Continuo)**
  - Delivery: Preparar para producción
  - Deployment: Desplegar a producción
  - Automatizar el proceso completo

### Beneficios
1. Detección temprana de errores
2. Entregas más rápidas
3. Procesos consistentes
4. Mayor calidad del código

> **Nota sobre CI/CD:**
> - CI/CD es una práctica, no solo herramientas
> - Requiere automatización robusta
> - Las pruebas son fundamentales
> - Cada paso debe ser reproducible

## 2. Configuración Básica con GitHub Actions

### Estructura del Proyecto
```plaintext
mi-proyecto/
├── src/
│   └── app.js
├── tests/
│   └── app.test.js
├── .github/
│   └── workflows/
│       └── ci.yml
└── package.json
```

### Workflow Básico
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

### Package.json
```json
{
  "name": "mi-proyecto",
  "version": "1.0.0",
  "scripts": {
    "test": "jest",
    "build": "webpack",
    "start": "node src/app.js"
  },
  "devDependencies": {
    "jest": "^27.0.0",
    "webpack": "^5.0.0"
  }
}
```

> **Nota sobre GitHub Actions:**
> - Los workflows se definen en YAML
> - Se ejecutan en respuesta a eventos
> - Pueden tener múltiples jobs
> - Los jobs corren en paralelo por defecto

## 3. Pruebas Automatizadas

```javascript
// src/app.js
function sumar(a, b) {
    return a + b;
}

function multiplicar(a, b) {
    return a * b;
}

module.exports = { sumar, multiplicar };

// tests/app.test.js
const { sumar, multiplicar } = require('../src/app');

describe('Operaciones matemáticas', () => {
    test('suma dos números correctamente', () => {
        expect(sumar(2, 3)).toBe(5);
        expect(sumar(-1, 1)).toBe(0);
    });

    test('multiplica dos números correctamente', () => {
        expect(multiplicar(2, 3)).toBe(6);
        expect(multiplicar(-2, 3)).toBe(-6);
    });
});
```

> **Nota sobre Pruebas:**
> - Unitarias: Probar funciones individuales
> - Integración: Probar componentes juntos
> - E2E: Probar flujos completos
> - Las pruebas deben ser confiables

## 4. Pipeline Completo

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'

    - name: Install dependencies
      run: npm install

    - name: Run linter
      run: npm run lint

    - name: Run tests
      run: npm test

    - name: Build
      run: npm run build

    - name: Upload build artifacts
      uses: actions/upload-artifact@v2
      with:
        name: build
        path: dist/

  deploy:
    needs: build-and-test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest

    steps:
    - name: Download build artifacts
      uses: actions/download-artifact@v2
      with:
        name: build

    - name: Deploy to staging
      run: |
        echo "Deploying to staging..."
        # Aquí irían los comandos de despliegue

    - name: Run integration tests
      run: |
        echo "Running integration tests..."
        # Aquí irían las pruebas de integración

    - name: Deploy to production
      if: success()
      run: |
        echo "Deploying to production..."
        # Aquí irían los comandos de despliegue a producción
```

> **Nota sobre Pipeline:**
> - Dividir en etapas claras
> - Usar condiciones para despliegues
> - Mantener artefactos entre jobs
> - Implementar rollback automático

## 5. Mejores Prácticas

### 1. Control de Versiones
```bash
# Usar ramas para features
git checkout -b feature/nueva-funcionalidad

# Commits descriptivos
git commit -m "feat: agregar validación de formulario

- Agregar validación de email
- Mostrar mensajes de error
- Implementar tests"

# Pull requests
# - Usar templates
# - Requerir revisiones
# - Bloquear merge si fallan pruebas
```

### 2. Seguridad
```yaml
# Escaneo de seguridad
- name: Security scan
  uses: github/codeql-action/analyze@v1

# Secretos seguros
env:
  API_KEY: ${{ secrets.API_KEY }}
```

### 3. Monitoreo
```javascript
// Logging estructurado
console.log(JSON.stringify({
    level: 'info',
    message: 'Deployment successful',
    environment: 'production',
    version: '1.2.3',
    timestamp: new Date().toISOString()
}));
```

## Herramientas Populares

1. **Servidores CI/CD**
   - GitHub Actions
   - Jenkins
   - GitLab CI
   - CircleCI

2. **Testing**
   - Jest
   - Mocha
   - Cypress
   - Selenium

3. **Calidad de Código**
   - ESLint
   - SonarQube
   - CodeClimate

4. **Despliegue**
   - Docker
   - Kubernetes
   - Heroku
   - AWS/GCP/Azure

## Ejercicio Práctico

1. Crear una aplicación Node.js simple
2. Configurar GitHub Actions
3. Implementar pruebas automatizadas
4. Crear pipeline de despliegue
5. Configurar entornos (staging/production)

```javascript
// Ejemplo de aplicación Express simple
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.json({ message: 'Hello CI/CD!' });
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});

module.exports = app;
```

## Recursos Adicionales
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [CI/CD Best Practices](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [The Twelve-Factor App](https://12factor.net/)

¡Felicitaciones! 🎉 Has completado la introducción a CI/CD. Recuerda que la automatización es clave para un pipeline exitoso.
