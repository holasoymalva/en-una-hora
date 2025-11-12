# Curso Práctico de Node.js - Nivel Semi-Senior

## 📋 Descripción del Proyecto

Este curso te guiará en la construcción de una **API REST completa para un sistema de gestión de tareas (Task Management System)** utilizando Node.js con conceptos avanzados que son esenciales para desarrolladores semi-senior y que frecuentemente aparecen en entrevistas técnicas.

### 🎯 Objetivos del Curso

- Dominar arquitecturas escalables en Node.js
- Implementar patrones de diseño profesionales
- Manejar autenticación y autorización robusta
- Aplicar testing avanzado
- Optimizar rendimiento y seguridad
- Prepararte para entrevistas técnicas

---

## 🛠️ Stack Tecnológico

- **Runtime**: Node.js v18+
- **Framework**: Express.js
- **Base de Datos**: PostgreSQL + Redis (caché)
- **ORM**: Prisma
- **Autenticación**: JWT + Refresh Tokens
- **Validación**: Zod
- **Testing**: Jest + Supertest
- **Documentación**: Swagger/OpenAPI
- **Logging**: Winston
- **Monitoreo**: Prometheus + Grafana (opcional)

---

## 📚 Tabla de Contenidos

1. [Configuración Inicial del Proyecto](#1-configuración-inicial-del-proyecto)
2. [Arquitectura y Estructura de Carpetas](#2-arquitectura-y-estructura-de-carpetas)
3. [Configuración de Base de Datos con Prisma](#3-configuración-de-base-de-datos-con-prisma)
4. [Implementación de Capas de la Aplicación](#4-implementación-de-capas-de-la-aplicación)
5. [Sistema de Autenticación Avanzado](#5-sistema-de-autenticación-avanzado)
6. [Manejo de Errores y Logging](#6-manejo-de-errores-y-logging)
7. [Validación y Sanitización de Datos](#7-validación-y-sanitización-de-datos)
8. [Implementación de Caché con Redis](#8-implementación-de-caché-con-redis)
9. [Testing Completo](#9-testing-completo)
10. [Seguridad y Mejores Prácticas](#10-seguridad-y-mejores-prácticas)
11. [Documentación con Swagger](#11-documentación-con-swagger)
12. [Optimización y Performance](#12-optimización-y-performance)
13. [Deployment y CI/CD](#13-deployment-y-cicd)

---


## 1. Configuración Inicial del Proyecto

### 1.1 Prerequisitos

Asegúrate de tener instalado:
```bash
node --version  # v18.0.0 o superior
npm --version   # v9.0.0 o superior
```

### 1.2 Inicializar el Proyecto

```bash
# Crear directorio del proyecto
mkdir task-management-api
cd task-management-api

# Inicializar npm
npm init -y

# Instalar dependencias principales
npm install express dotenv cors helmet compression
npm install prisma @prisma/client
npm install jsonwebtoken bcryptjs
npm install zod
npm install winston
npm install ioredis

# Instalar dependencias de desarrollo
npm install -D typescript @types/node @types/express
npm install -D ts-node nodemon
npm install -D jest @types/jest ts-jest supertest @types/supertest
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
npm install -D prettier eslint-config-prettier
npm install -D swagger-jsdoc swagger-ui-express @types/swagger-jsdoc @types/swagger-ui-express
```

### 1.3 Configurar TypeScript

Crear `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "lib": ["ES2022"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "moduleResolution": "node",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

### 1.4 Configurar Scripts en package.json

Actualizar la sección `scripts`:
```json
{
  "scripts": {
    "dev": "nodemon --exec ts-node src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "test": "jest --coverage",
    "test:watch": "jest --watch",
    "lint": "eslint . --ext .ts",
    "lint:fix": "eslint . --ext .ts --fix",
    "format": "prettier --write \"src/**/*.ts\"",
    "prisma:generate": "prisma generate",
    "prisma:migrate": "prisma migrate dev",
    "prisma:studio": "prisma studio"
  }
}
```

### 1.5 Configurar ESLint

Crear `.eslintrc.json`:
```json
{
  "parser": "@typescript-eslint/parser",
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "plugins": ["@typescript-eslint"],
  "env": {
    "node": true,
    "es6": true
  },
  "rules": {
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/no-unused-vars": ["error", { "argsIgnorePattern": "^_" }]
  }
}
```

### 1.6 Configurar Prettier

Crear `.prettierrc`:
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "arrowParens": "always"
}
```

### 1.7 Variables de Entorno

Crear `.env`:
```env
# Server
NODE_ENV=development
PORT=3000
API_VERSION=v1

# Database
DATABASE_URL="postgresql://user:password@localhost:5432/taskdb?schema=public"

# JWT
JWT_SECRET=your-super-secret-jwt-key-change-in-production
JWT_REFRESH_SECRET=your-super-secret-refresh-key-change-in-production
JWT_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# Logging
LOG_LEVEL=debug
```

Crear `.env.example` (sin valores sensibles):
```env
NODE_ENV=development
PORT=3000
DATABASE_URL=
JWT_SECRET=
JWT_REFRESH_SECRET=
REDIS_HOST=localhost
REDIS_PORT=6379
```

### 1.8 Crear .gitignore

```
node_modules/
dist/
.env
.env.local
.env.*.local
coverage/
*.log
.DS_Store
.vscode/
.idea/
```

---


## 2. Arquitectura y Estructura de Carpetas

### 2.1 Patrón de Arquitectura: Clean Architecture + Layered

Utilizaremos una arquitectura en capas que separa responsabilidades:

```
src/
├── config/              # Configuraciones (DB, Redis, etc.)
│   ├── database.ts
│   ├── redis.ts
│   └── logger.ts
├── controllers/         # Controladores (manejo de requests/responses)
│   ├── auth.controller.ts
│   ├── task.controller.ts
│   └── user.controller.ts
├── services/           # Lógica de negocio
│   ├── auth.service.ts
│   ├── task.service.ts
│   └── user.service.ts
├── repositories/       # Acceso a datos (abstracción de Prisma)
│   ├── task.repository.ts
│   └── user.repository.ts
├── middlewares/        # Middlewares personalizados
│   ├── auth.middleware.ts
│   ├── error.middleware.ts
│   ├── validation.middleware.ts
│   └── rateLimit.middleware.ts
├── routes/             # Definición de rutas
│   ├── auth.routes.ts
│   ├── task.routes.ts
│   ├── user.routes.ts
│   └── index.ts
├── models/             # Interfaces y tipos TypeScript
│   ├── user.model.ts
│   ├── task.model.ts
│   └── common.model.ts
├── validators/         # Esquemas de validación con Zod
│   ├── auth.validator.ts
│   ├── task.validator.ts
│   └── user.validator.ts
├── utils/              # Utilidades y helpers
│   ├── jwt.util.ts
│   ├── password.util.ts
│   ├── response.util.ts
│   └── asyncHandler.util.ts
├── errors/             # Clases de errores personalizados
│   ├── AppError.ts
│   ├── ValidationError.ts
│   └── AuthenticationError.ts
├── types/              # Tipos globales y extensiones
│   └── express.d.ts
├── docs/               # Documentación Swagger
│   └── swagger.ts
├── tests/              # Tests
│   ├── unit/
│   ├── integration/
│   └── helpers/
├── app.ts              # Configuración de Express
└── server.ts           # Punto de entrada

prisma/
├── schema.prisma       # Esquema de base de datos
└── migrations/         # Migraciones
```

### 2.2 Principios de la Arquitectura

**Separación de Responsabilidades:**
- **Controllers**: Manejan HTTP (request/response), no contienen lógica de negocio
- **Services**: Contienen toda la lógica de negocio
- **Repositories**: Abstraen el acceso a datos (facilita cambiar de ORM)
- **Middlewares**: Procesan requests antes de llegar a controllers
- **Validators**: Validan y sanitizan datos de entrada

**Ventajas de esta arquitectura:**
- ✅ Testeable: Cada capa se puede testear independientemente
- ✅ Mantenible: Cambios en una capa no afectan otras
- ✅ Escalable: Fácil agregar nuevas funcionalidades
- ✅ Reutilizable: Services y repositories se pueden usar en diferentes contextos

### 2.3 Crear Estructura de Carpetas

```bash
mkdir -p src/{config,controllers,services,repositories,middlewares,routes,models,validators,utils,errors,types,docs,tests/{unit,integration,helpers}}
mkdir -p prisma
```

---


## 3. Configuración de Base de Datos con Prisma

### 3.1 Inicializar Prisma

```bash
npx prisma init
```

### 3.2 Definir el Esquema de Base de Datos

Editar `prisma/schema.prisma`:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

enum UserRole {
  USER
  ADMIN
  MANAGER
}

enum TaskStatus {
  TODO
  IN_PROGRESS
  IN_REVIEW
  DONE
  CANCELLED
}

enum TaskPriority {
  LOW
  MEDIUM
  HIGH
  URGENT
}

model User {
  id            String    @id @default(uuid())
  email         String    @unique
  username      String    @unique
  password      String
  firstName     String?
  lastName      String?
  role          UserRole  @default(USER)
  isActive      Boolean   @default(true)
  emailVerified Boolean   @default(false)
  
  // Relaciones
  tasks         Task[]    @relation("TaskOwner")
  assignedTasks Task[]    @relation("TaskAssignee")
  comments      Comment[]
  refreshTokens RefreshToken[]
  
  // Timestamps
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  lastLoginAt   DateTime?
  
  @@index([email])
  @@index([username])
  @@map("users")
}

model Task {
  id          String       @id @default(uuid())
  title       String
  description String?      @db.Text
  status      TaskStatus   @default(TODO)
  priority    TaskPriority @default(MEDIUM)
  dueDate     DateTime?
  
  // Relaciones
  ownerId     String
  owner       User         @relation("TaskOwner", fields: [ownerId], references: [id], onDelete: Cascade)
  
  assigneeId  String?
  assignee    User?        @relation("TaskAssignee", fields: [assigneeId], references: [id], onDelete: SetNull)
  
  tags        Tag[]
  comments    Comment[]
  attachments Attachment[]
  
  // Timestamps
  createdAt   DateTime     @default(now())
  updatedAt   DateTime     @updatedAt
  completedAt DateTime?
  
  @@index([ownerId])
  @@index([assigneeId])
  @@index([status])
  @@index([priority])
  @@index([dueDate])
  @@map("tasks")
}

model Tag {
  id        String   @id @default(uuid())
  name      String   @unique
  color     String?
  
  tasks     Task[]
  
  createdAt DateTime @default(now())
  
  @@map("tags")
}

model Comment {
  id        String   @id @default(uuid())
  content   String   @db.Text
  
  taskId    String
  task      Task     @relation(fields: [taskId], references: [id], onDelete: Cascade)
  
  authorId  String
  author    User     @relation(fields: [authorId], references: [id], onDelete: Cascade)
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  
  @@index([taskId])
  @@index([authorId])
  @@map("comments")
}

model Attachment {
  id        String   @id @default(uuid())
  filename  String
  url       String
  mimeType  String
  size      Int
  
  taskId    String
  task      Task     @relation(fields: [taskId], references: [id], onDelete: Cascade)
  
  createdAt DateTime @default(now())
  
  @@index([taskId])
  @@map("attachments")
}

model RefreshToken {
  id        String   @id @default(uuid())
  token     String   @unique
  
  userId    String
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  expiresAt DateTime
  createdAt DateTime @default(now())
  
  @@index([userId])
  @@index([token])
  @@map("refresh_tokens")
}
```

### 3.3 Crear y Ejecutar Migraciones

```bash
# Crear migración inicial
npx prisma migrate dev --name init

# Generar cliente de Prisma
npx prisma generate

# Ver base de datos en navegador (opcional)
npx prisma studio
```

### 3.4 Configurar Cliente de Prisma

Crear `src/config/database.ts`:

```typescript
import { PrismaClient } from '@prisma/client';
import logger from './logger';

// Singleton pattern para Prisma Client
class Database {
  private static instance: PrismaClient;

  private constructor() {}

  public static getInstance(): PrismaClient {
    if (!Database.instance) {
      Database.instance = new PrismaClient({
        log: [
          { level: 'query', emit: 'event' },
          { level: 'error', emit: 'stdout' },
          { level: 'warn', emit: 'stdout' },
        ],
      });

      // Log de queries en desarrollo
      if (process.env.NODE_ENV === 'development') {
        Database.instance.$on('query' as never, (e: any) => {
          logger.debug('Query: ' + e.query);
          logger.debug('Duration: ' + e.duration + 'ms');
        });
      }

      // Manejo de desconexión
      process.on('beforeExit', async () => {
        await Database.instance.$disconnect();
        logger.info('Database disconnected');
      });
    }

    return Database.instance;
  }

  public static async connect(): Promise<void> {
    try {
      const prisma = Database.getInstance();
      await prisma.$connect();
      logger.info('Database connected successfully');
    } catch (error) {
      logger.error('Database connection failed:', error);
      process.exit(1);
    }
  }

  public static async disconnect(): Promise<void> {
    const prisma = Database.getInstance();
    await prisma.$disconnect();
  }
}

export const prisma = Database.getInstance();
export default Database;
```

### 3.5 Conceptos Importantes de Prisma

**¿Por qué Prisma?**
- ✅ Type-safe: TypeScript de primera clase
- ✅ Auto-completion: IntelliSense completo
- ✅ Migraciones: Control de versiones de DB
- ✅ Prisma Studio: GUI para ver datos
- ✅ Performance: Queries optimizadas

**Patrones comunes en entrevistas:**
- Singleton pattern para cliente de DB
- Connection pooling automático
- Manejo de transacciones
- Soft deletes vs Hard deletes

---


## 4. Implementación de Capas de la Aplicación

### 4.1 Modelos y Tipos TypeScript

Crear `src/models/common.model.ts`:

```typescript
export interface PaginationParams {
  page: number;
  limit: number;
  sortBy?: string;
  sortOrder?: 'asc' | 'desc';
}

export interface PaginatedResponse<T> {
  data: T[];
  pagination: {
    page: number;
    limit: number;
    total: number;
    totalPages: number;
    hasNext: boolean;
    hasPrev: boolean;
  };
}

export interface ApiResponse<T = any> {
  success: boolean;
  message?: string;
  data?: T;
  error?: string;
}
```

Crear `src/models/user.model.ts`:

```typescript
import { UserRole } from '@prisma/client';

export interface CreateUserDTO {
  email: string;
  username: string;
  password: string;
  firstName?: string;
  lastName?: string;
  role?: UserRole;
}

export interface UpdateUserDTO {
  email?: string;
  username?: string;
  firstName?: string;
  lastName?: string;
  role?: UserRole;
  isActive?: boolean;
}

export interface UserResponse {
  id: string;
  email: string;
  username: string;
  firstName?: string;
  lastName?: string;
  role: UserRole;
  isActive: boolean;
  createdAt: Date;
}

export interface LoginDTO {
  email: string;
  password: string;
}

export interface AuthResponse {
  user: UserResponse;
  accessToken: string;
  refreshToken: string;
}
```

Crear `src/models/task.model.ts`:

```typescript
import { TaskStatus, TaskPriority } from '@prisma/client';

export interface CreateTaskDTO {
  title: string;
  description?: string;
  status?: TaskStatus;
  priority?: TaskPriority;
  dueDate?: Date;
  assigneeId?: string;
  tags?: string[];
}

export interface UpdateTaskDTO {
  title?: string;
  description?: string;
  status?: TaskStatus;
  priority?: TaskPriority;
  dueDate?: Date;
  assigneeId?: string;
  tags?: string[];
}

export interface TaskFilterDTO {
  status?: TaskStatus;
  priority?: TaskPriority;
  assigneeId?: string;
  ownerId?: string;
  tags?: string[];
  dueDateFrom?: Date;
  dueDateTo?: Date;
  search?: string;
}
```

### 4.2 Clases de Errores Personalizados

Crear `src/errors/AppError.ts`:

```typescript
export class AppError extends Error {
  public readonly statusCode: number;
  public readonly isOperational: boolean;

  constructor(message: string, statusCode: number = 500, isOperational: boolean = true) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = isOperational;

    Error.captureStackTrace(this, this.constructor);
  }
}

export class ValidationError extends AppError {
  constructor(message: string = 'Validation failed') {
    super(message, 400);
  }
}

export class AuthenticationError extends AppError {
  constructor(message: string = 'Authentication failed') {
    super(message, 401);
  }
}

export class AuthorizationError extends AppError {
  constructor(message: string = 'Access denied') {
    super(message, 403);
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string = 'Resource') {
    super(`${resource} not found`, 404);
  }
}

export class ConflictError extends AppError {
  constructor(message: string = 'Resource already exists') {
    super(message, 409);
  }
}

export class RateLimitError extends AppError {
  constructor(message: string = 'Too many requests') {
    super(message, 429);
  }
}
```

### 4.3 Utilidades

Crear `src/utils/asyncHandler.util.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';

// Wrapper para manejar errores en funciones async
export const asyncHandler = (fn: Function) => {
  return (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
};
```

Crear `src/utils/response.util.ts`:

```typescript
import { Response } from 'express';
import { ApiResponse } from '../models/common.model';

export class ResponseUtil {
  static success<T>(res: Response, data: T, message?: string, statusCode: number = 200) {
    const response: ApiResponse<T> = {
      success: true,
      message,
      data,
    };
    return res.status(statusCode).json(response);
  }

  static error(res: Response, error: string, statusCode: number = 500) {
    const response: ApiResponse = {
      success: false,
      error,
    };
    return res.status(statusCode).json(response);
  }

  static created<T>(res: Response, data: T, message: string = 'Resource created successfully') {
    return this.success(res, data, message, 201);
  }

  static noContent(res: Response) {
    return res.status(204).send();
  }
}
```

Crear `src/utils/password.util.ts`:

```typescript
import bcrypt from 'bcryptjs';

export class PasswordUtil {
  private static readonly SALT_ROUNDS = 12;

  static async hash(password: string): Promise<string> {
    return bcrypt.hash(password, this.SALT_ROUNDS);
  }

  static async compare(password: string, hashedPassword: string): Promise<boolean> {
    return bcrypt.compare(password, hashedPassword);
  }

  static validate(password: string): { valid: boolean; errors: string[] } {
    const errors: string[] = [];

    if (password.length < 8) {
      errors.push('Password must be at least 8 characters long');
    }
    if (!/[A-Z]/.test(password)) {
      errors.push('Password must contain at least one uppercase letter');
    }
    if (!/[a-z]/.test(password)) {
      errors.push('Password must contain at least one lowercase letter');
    }
    if (!/[0-9]/.test(password)) {
      errors.push('Password must contain at least one number');
    }
    if (!/[!@#$%^&*]/.test(password)) {
      errors.push('Password must contain at least one special character (!@#$%^&*)');
    }

    return {
      valid: errors.length === 0,
      errors,
    };
  }
}
```

Crear `src/utils/jwt.util.ts`:

```typescript
import jwt from 'jsonwebtoken';
import { AuthenticationError } from '../errors/AppError';

export interface JWTPayload {
  userId: string;
  email: string;
  role: string;
}

export class JWTUtil {
  private static readonly ACCESS_SECRET = process.env.JWT_SECRET!;
  private static readonly REFRESH_SECRET = process.env.JWT_REFRESH_SECRET!;
  private static readonly ACCESS_EXPIRES_IN = process.env.JWT_EXPIRES_IN || '15m';
  private static readonly REFRESH_EXPIRES_IN = process.env.JWT_REFRESH_EXPIRES_IN || '7d';

  static generateAccessToken(payload: JWTPayload): string {
    return jwt.sign(payload, this.ACCESS_SECRET, {
      expiresIn: this.ACCESS_EXPIRES_IN,
    });
  }

  static generateRefreshToken(payload: JWTPayload): string {
    return jwt.sign(payload, this.REFRESH_SECRET, {
      expiresIn: this.REFRESH_EXPIRES_IN,
    });
  }

  static verifyAccessToken(token: string): JWTPayload {
    try {
      return jwt.verify(token, this.ACCESS_SECRET) as JWTPayload;
    } catch (error) {
      throw new AuthenticationError('Invalid or expired access token');
    }
  }

  static verifyRefreshToken(token: string): JWTPayload {
    try {
      return jwt.verify(token, this.REFRESH_SECRET) as JWTPayload;
    } catch (error) {
      throw new AuthenticationError('Invalid or expired refresh token');
    }
  }

  static decodeToken(token: string): JWTPayload | null {
    try {
      return jwt.decode(token) as JWTPayload;
    } catch {
      return null;
    }
  }
}
```

---


## 5. Sistema de Autenticación Avanzado

### 5.1 Repository Layer

Crear `src/repositories/user.repository.ts`:

```typescript
import { prisma } from '../config/database';
import { User, UserRole } from '@prisma/client';
import { CreateUserDTO, UpdateUserDTO } from '../models/user.model';
import { PaginationParams } from '../models/common.model';

export class UserRepository {
  async create(data: CreateUserDTO): Promise<User> {
    return prisma.user.create({
      data,
    });
  }

  async findById(id: string): Promise<User | null> {
    return prisma.user.findUnique({
      where: { id },
      include: {
        tasks: {
          select: {
            id: true,
            title: true,
            status: true,
          },
        },
      },
    });
  }

  async findByEmail(email: string): Promise<User | null> {
    return prisma.user.findUnique({
      where: { email },
    });
  }

  async findByUsername(username: string): Promise<User | null> {
    return prisma.user.findUnique({
      where: { username },
    });
  }

  async findAll(params: PaginationParams) {
    const { page, limit, sortBy = 'createdAt', sortOrder = 'desc' } = params;
    const skip = (page - 1) * limit;

    const [users, total] = await Promise.all([
      prisma.user.findMany({
        skip,
        take: limit,
        orderBy: { [sortBy]: sortOrder },
        select: {
          id: true,
          email: true,
          username: true,
          firstName: true,
          lastName: true,
          role: true,
          isActive: true,
          createdAt: true,
        },
      }),
      prisma.user.count(),
    ]);

    return { users, total };
  }

  async update(id: string, data: UpdateUserDTO): Promise<User> {
    return prisma.user.update({
      where: { id },
      data,
    });
  }

  async delete(id: string): Promise<void> {
    await prisma.user.delete({
      where: { id },
    });
  }

  async updateLastLogin(id: string): Promise<void> {
    await prisma.user.update({
      where: { id },
      data: { lastLoginAt: new Date() },
    });
  }

  async saveRefreshToken(userId: string, token: string, expiresAt: Date): Promise<void> {
    await prisma.refreshToken.create({
      data: {
        userId,
        token,
        expiresAt,
      },
    });
  }

  async findRefreshToken(token: string) {
    return prisma.refreshToken.findUnique({
      where: { token },
      include: { user: true },
    });
  }

  async deleteRefreshToken(token: string): Promise<void> {
    await prisma.refreshToken.delete({
      where: { token },
    });
  }

  async deleteAllUserRefreshTokens(userId: string): Promise<void> {
    await prisma.refreshToken.deleteMany({
      where: { userId },
    });
  }
}
```

### 5.2 Service Layer

Crear `src/services/auth.service.ts`:

```typescript
import { UserRepository } from '../repositories/user.repository';
import { PasswordUtil } from '../utils/password.util';
import { JWTUtil, JWTPayload } from '../utils/jwt.util';
import {
  CreateUserDTO,
  LoginDTO,
  AuthResponse,
  UserResponse,
} from '../models/user.model';
import {
  AuthenticationError,
  ConflictError,
  ValidationError,
} from '../errors/AppError';

export class AuthService {
  private userRepository: UserRepository;

  constructor() {
    this.userRepository = new UserRepository();
  }

  async register(data: CreateUserDTO): Promise<AuthResponse> {
    // Validar password
    const passwordValidation = PasswordUtil.validate(data.password);
    if (!passwordValidation.valid) {
      throw new ValidationError(passwordValidation.errors.join(', '));
    }

    // Verificar si el usuario ya existe
    const existingUser = await this.userRepository.findByEmail(data.email);
    if (existingUser) {
      throw new ConflictError('Email already registered');
    }

    const existingUsername = await this.userRepository.findByUsername(data.username);
    if (existingUsername) {
      throw new ConflictError('Username already taken');
    }

    // Hash password
    const hashedPassword = await PasswordUtil.hash(data.password);

    // Crear usuario
    const user = await this.userRepository.create({
      ...data,
      password: hashedPassword,
    });

    // Generar tokens
    const tokens = this.generateTokens(user);

    // Guardar refresh token
    const refreshTokenExpiry = new Date();
    refreshTokenExpiry.setDate(refreshTokenExpiry.getDate() + 7);
    await this.userRepository.saveRefreshToken(
      user.id,
      tokens.refreshToken,
      refreshTokenExpiry
    );

    return {
      user: this.sanitizeUser(user),
      ...tokens,
    };
  }

  async login(data: LoginDTO): Promise<AuthResponse> {
    // Buscar usuario
    const user = await this.userRepository.findByEmail(data.email);
    if (!user) {
      throw new AuthenticationError('Invalid credentials');
    }

    // Verificar si está activo
    if (!user.isActive) {
      throw new AuthenticationError('Account is deactivated');
    }

    // Verificar password
    const isPasswordValid = await PasswordUtil.compare(data.password, user.password);
    if (!isPasswordValid) {
      throw new AuthenticationError('Invalid credentials');
    }

    // Actualizar último login
    await this.userRepository.updateLastLogin(user.id);

    // Generar tokens
    const tokens = this.generateTokens(user);

    // Guardar refresh token
    const refreshTokenExpiry = new Date();
    refreshTokenExpiry.setDate(refreshTokenExpiry.getDate() + 7);
    await this.userRepository.saveRefreshToken(
      user.id,
      tokens.refreshToken,
      refreshTokenExpiry
    );

    return {
      user: this.sanitizeUser(user),
      ...tokens,
    };
  }

  async refreshToken(refreshToken: string): Promise<{ accessToken: string; refreshToken: string }> {
    // Verificar refresh token
    const payload = JWTUtil.verifyRefreshToken(refreshToken);

    // Buscar token en DB
    const tokenRecord = await this.userRepository.findRefreshToken(refreshToken);
    if (!tokenRecord) {
      throw new AuthenticationError('Invalid refresh token');
    }

    // Verificar expiración
    if (new Date() > tokenRecord.expiresAt) {
      await this.userRepository.deleteRefreshToken(refreshToken);
      throw new AuthenticationError('Refresh token expired');
    }

    // Generar nuevos tokens
    const user = tokenRecord.user;
    const tokens = this.generateTokens(user);

    // Eliminar token viejo y guardar nuevo
    await this.userRepository.deleteRefreshToken(refreshToken);
    const newRefreshTokenExpiry = new Date();
    newRefreshTokenExpiry.setDate(newRefreshTokenExpiry.getDate() + 7);
    await this.userRepository.saveRefreshToken(
      user.id,
      tokens.refreshToken,
      newRefreshTokenExpiry
    );

    return tokens;
  }

  async logout(refreshToken: string): Promise<void> {
    await this.userRepository.deleteRefreshToken(refreshToken);
  }

  async logoutAll(userId: string): Promise<void> {
    await this.userRepository.deleteAllUserRefreshTokens(userId);
  }

  private generateTokens(user: any): { accessToken: string; refreshToken: string } {
    const payload: JWTPayload = {
      userId: user.id,
      email: user.email,
      role: user.role,
    };

    return {
      accessToken: JWTUtil.generateAccessToken(payload),
      refreshToken: JWTUtil.generateRefreshToken(payload),
    };
  }

  private sanitizeUser(user: any): UserResponse {
    const { password, ...sanitized } = user;
    return sanitized;
  }
}
```

### 5.3 Validators con Zod

Crear `src/validators/auth.validator.ts`:

```typescript
import { z } from 'zod';

export const registerSchema = z.object({
  body: z.object({
    email: z.string().email('Invalid email format'),
    username: z
      .string()
      .min(3, 'Username must be at least 3 characters')
      .max(30, 'Username must not exceed 30 characters')
      .regex(/^[a-zA-Z0-9_]+$/, 'Username can only contain letters, numbers, and underscores'),
    password: z
      .string()
      .min(8, 'Password must be at least 8 characters')
      .regex(/[A-Z]/, 'Password must contain at least one uppercase letter')
      .regex(/[a-z]/, 'Password must contain at least one lowercase letter')
      .regex(/[0-9]/, 'Password must contain at least one number')
      .regex(/[!@#$%^&*]/, 'Password must contain at least one special character'),
    firstName: z.string().optional(),
    lastName: z.string().optional(),
  }),
});

export const loginSchema = z.object({
  body: z.object({
    email: z.string().email('Invalid email format'),
    password: z.string().min(1, 'Password is required'),
  }),
});

export const refreshTokenSchema = z.object({
  body: z.object({
    refreshToken: z.string().min(1, 'Refresh token is required'),
  }),
});
```

### 5.4 Middleware de Validación

Crear `src/middlewares/validation.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import { AnyZodObject, ZodError } from 'zod';
import { ValidationError } from '../errors/AppError';

export const validate = (schema: AnyZodObject) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    try {
      await schema.parseAsync({
        body: req.body,
        query: req.query,
        params: req.params,
      });
      next();
    } catch (error) {
      if (error instanceof ZodError) {
        const errors = error.errors.map((err) => ({
          field: err.path.join('.'),
          message: err.message,
        }));
        next(new ValidationError(JSON.stringify(errors)));
      } else {
        next(error);
      }
    }
  };
};
```

---


### 5.5 Middleware de Autenticación

Crear `src/types/express.d.ts`:

```typescript
import { JWTPayload } from '../utils/jwt.util';

declare global {
  namespace Express {
    interface Request {
      user?: JWTPayload;
    }
  }
}
```

Crear `src/middlewares/auth.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import { JWTUtil } from '../utils/jwt.util';
import { AuthenticationError, AuthorizationError } from '../errors/AppError';
import { UserRole } from '@prisma/client';

export const authenticate = (req: Request, res: Response, next: NextFunction) => {
  try {
    // Obtener token del header
    const authHeader = req.headers.authorization;
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      throw new AuthenticationError('No token provided');
    }

    const token = authHeader.substring(7);

    // Verificar token
    const payload = JWTUtil.verifyAccessToken(token);

    // Agregar usuario al request
    req.user = payload;

    next();
  } catch (error) {
    next(error);
  }
};

export const authorize = (...roles: UserRole[]) => {
  return (req: Request, res: Response, next: NextFunction) => {
    if (!req.user) {
      return next(new AuthenticationError('User not authenticated'));
    }

    if (!roles.includes(req.user.role as UserRole)) {
      return next(new AuthorizationError('Insufficient permissions'));
    }

    next();
  };
};

// Middleware opcional: permite acceso sin token pero agrega user si existe
export const optionalAuth = (req: Request, res: Response, next: NextFunction) => {
  try {
    const authHeader = req.headers.authorization;
    if (authHeader && authHeader.startsWith('Bearer ')) {
      const token = authHeader.substring(7);
      const payload = JWTUtil.verifyAccessToken(token);
      req.user = payload;
    }
    next();
  } catch (error) {
    // Ignorar errores de token en auth opcional
    next();
  }
};
```

### 5.6 Controller Layer

Crear `src/controllers/auth.controller.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import { AuthService } from '../services/auth.service';
import { ResponseUtil } from '../utils/response.util';
import { asyncHandler } from '../utils/asyncHandler.util';

export class AuthController {
  private authService: AuthService;

  constructor() {
    this.authService = new AuthService();
  }

  register = asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    const result = await this.authService.register(req.body);
    return ResponseUtil.created(res, result, 'User registered successfully');
  });

  login = asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    const result = await this.authService.login(req.body);
    return ResponseUtil.success(res, result, 'Login successful');
  });

  refreshToken = asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    const { refreshToken } = req.body;
    const result = await this.authService.refreshToken(refreshToken);
    return ResponseUtil.success(res, result, 'Token refreshed successfully');
  });

  logout = asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    const { refreshToken } = req.body;
    await this.authService.logout(refreshToken);
    return ResponseUtil.success(res, null, 'Logout successful');
  });

  logoutAll = asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    const userId = req.user!.userId;
    await this.authService.logoutAll(userId);
    return ResponseUtil.success(res, null, 'Logged out from all devices');
  });

  me = asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    return ResponseUtil.success(res, req.user, 'User profile retrieved');
  });
}
```

### 5.7 Routes

Crear `src/routes/auth.routes.ts`:

```typescript
import { Router } from 'express';
import { AuthController } from '../controllers/auth.controller';
import { validate } from '../middlewares/validation.middleware';
import { authenticate } from '../middlewares/auth.middleware';
import { registerSchema, loginSchema, refreshTokenSchema } from '../validators/auth.validator';

const router = Router();
const authController = new AuthController();

/**
 * @route   POST /api/v1/auth/register
 * @desc    Register a new user
 * @access  Public
 */
router.post('/register', validate(registerSchema), authController.register);

/**
 * @route   POST /api/v1/auth/login
 * @desc    Login user
 * @access  Public
 */
router.post('/login', validate(loginSchema), authController.login);

/**
 * @route   POST /api/v1/auth/refresh
 * @desc    Refresh access token
 * @access  Public
 */
router.post('/refresh', validate(refreshTokenSchema), authController.refreshToken);

/**
 * @route   POST /api/v1/auth/logout
 * @desc    Logout user (invalidate refresh token)
 * @access  Private
 */
router.post('/logout', authenticate, authController.logout);

/**
 * @route   POST /api/v1/auth/logout-all
 * @desc    Logout from all devices
 * @access  Private
 */
router.post('/logout-all', authenticate, authController.logoutAll);

/**
 * @route   GET /api/v1/auth/me
 * @desc    Get current user profile
 * @access  Private
 */
router.get('/me', authenticate, authController.me);

export default router;
```

---


## 6. Manejo de Errores y Logging

### 6.1 Configurar Winston Logger

Crear `src/config/logger.ts`:

```typescript
import winston from 'winston';
import path from 'path';

const logDir = 'logs';

// Definir niveles de log personalizados
const levels = {
  error: 0,
  warn: 1,
  info: 2,
  http: 3,
  debug: 4,
};

// Definir colores para cada nivel
const colors = {
  error: 'red',
  warn: 'yellow',
  info: 'green',
  http: 'magenta',
  debug: 'blue',
};

winston.addColors(colors);

// Formato para logs
const format = winston.format.combine(
  winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
  winston.format.errors({ stack: true }),
  winston.format.splat(),
  winston.format.json()
);

// Formato para consola (desarrollo)
const consoleFormat = winston.format.combine(
  winston.format.colorize({ all: true }),
  winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
  winston.format.printf((info) => {
    const { timestamp, level, message, ...meta } = info;
    return `${timestamp} [${level}]: ${message} ${
      Object.keys(meta).length ? JSON.stringify(meta, null, 2) : ''
    }`;
  })
);

// Transports
const transports = [
  // Console transport
  new winston.transports.Console({
    format: consoleFormat,
  }),
  // Error log file
  new winston.transports.File({
    filename: path.join(logDir, 'error.log'),
    level: 'error',
    format,
  }),
  // Combined log file
  new winston.transports.File({
    filename: path.join(logDir, 'combined.log'),
    format,
  }),
];

// Crear logger
const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  levels,
  format,
  transports,
  exitOnError: false,
});

// Stream para Morgan (HTTP logging)
export const stream = {
  write: (message: string) => {
    logger.http(message.trim());
  },
};

export default logger;
```

### 6.2 Middleware de Manejo de Errores

Crear `src/middlewares/error.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import { AppError } from '../errors/AppError';
import logger from '../config/logger';
import { Prisma } from '@prisma/client';

export const errorHandler = (
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  // Log del error
  logger.error('Error:', {
    message: err.message,
    stack: err.stack,
    url: req.url,
    method: req.method,
    ip: req.ip,
  });

  // Error operacional conocido
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      success: false,
      error: err.message,
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack }),
    });
  }

  // Errores de Prisma
  if (err instanceof Prisma.PrismaClientKnownRequestError) {
    return handlePrismaError(err, res);
  }

  if (err instanceof Prisma.PrismaClientValidationError) {
    return res.status(400).json({
      success: false,
      error: 'Invalid data provided',
      ...(process.env.NODE_ENV === 'development' && { details: err.message }),
    });
  }

  // Error de validación de Zod (por si acaso pasa el middleware)
  if (err.name === 'ZodError') {
    return res.status(400).json({
      success: false,
      error: 'Validation failed',
      details: err,
    });
  }

  // Error desconocido
  return res.status(500).json({
    success: false,
    error: process.env.NODE_ENV === 'production' 
      ? 'Internal server error' 
      : err.message,
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack }),
  });
};

// Manejar errores específicos de Prisma
const handlePrismaError = (err: Prisma.PrismaClientKnownRequestError, res: Response) => {
  switch (err.code) {
    case 'P2002':
      // Unique constraint violation
      return res.status(409).json({
        success: false,
        error: 'Resource already exists',
        field: err.meta?.target,
      });
    case 'P2025':
      // Record not found
      return res.status(404).json({
        success: false,
        error: 'Resource not found',
      });
    case 'P2003':
      // Foreign key constraint violation
      return res.status(400).json({
        success: false,
        error: 'Invalid reference to related resource',
      });
    default:
      return res.status(500).json({
        success: false,
        error: 'Database error',
        ...(process.env.NODE_ENV === 'development' && { code: err.code }),
      });
  }
};

// Middleware para rutas no encontradas
export const notFoundHandler = (req: Request, res: Response, next: NextFunction) => {
  res.status(404).json({
    success: false,
    error: `Route ${req.originalUrl} not found`,
  });
};

// Manejador de errores no capturados
export const setupGlobalErrorHandlers = () => {
  process.on('unhandledRejection', (reason: Error) => {
    logger.error('Unhandled Rejection:', reason);
    // En producción, podrías querer cerrar el servidor gracefully
    if (process.env.NODE_ENV === 'production') {
      process.exit(1);
    }
  });

  process.on('uncaughtException', (error: Error) => {
    logger.error('Uncaught Exception:', error);
    // Siempre salir en uncaught exceptions
    process.exit(1);
  });
};
```

### 6.3 Middleware de Request Logging

Crear `src/middlewares/requestLogger.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import logger from '../config/logger';

export const requestLogger = (req: Request, res: Response, next: NextFunction) => {
  const start = Date.now();

  // Log cuando la respuesta termina
  res.on('finish', () => {
    const duration = Date.now() - start;
    const logData = {
      method: req.method,
      url: req.originalUrl,
      status: res.statusCode,
      duration: `${duration}ms`,
      ip: req.ip,
      userAgent: req.get('user-agent'),
      userId: req.user?.userId || 'anonymous',
    };

    if (res.statusCode >= 400) {
      logger.warn('Request completed with error', logData);
    } else {
      logger.http('Request completed', logData);
    }
  });

  next();
};
```

### 6.4 Rate Limiting Middleware

Crear `src/middlewares/rateLimit.middleware.ts`:

```typescript
import rateLimit from 'express-rate-limit';
import { RateLimitError } from '../errors/AppError';

// Rate limiter general
export const generalLimiter = rateLimit({
  windowMs: parseInt(process.env.RATE_LIMIT_WINDOW_MS || '900000'), // 15 minutos
  max: parseInt(process.env.RATE_LIMIT_MAX_REQUESTS || '100'),
  message: 'Too many requests from this IP, please try again later',
  standardHeaders: true,
  legacyHeaders: false,
  handler: (req, res) => {
    throw new RateLimitError();
  },
});

// Rate limiter estricto para autenticación
export const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5, // 5 intentos
  skipSuccessfulRequests: true,
  message: 'Too many authentication attempts, please try again later',
  handler: (req, res) => {
    throw new RateLimitError('Too many authentication attempts');
  },
});

// Rate limiter para creación de recursos
export const createLimiter = rateLimit({
  windowMs: 60 * 1000, // 1 minuto
  max: 10, // 10 creaciones por minuto
  message: 'Too many resources created, please slow down',
  handler: (req, res) => {
    throw new RateLimitError('Too many resources created');
  },
});
```

---


## 7. Validación y Sanitización de Datos

### 7.1 Validators para Tasks

Crear `src/validators/task.validator.ts`:

```typescript
import { z } from 'zod';
import { TaskStatus, TaskPriority } from '@prisma/client';

export const createTaskSchema = z.object({
  body: z.object({
    title: z
      .string()
      .min(3, 'Title must be at least 3 characters')
      .max(200, 'Title must not exceed 200 characters'),
    description: z.string().max(2000, 'Description too long').optional(),
    status: z.nativeEnum(TaskStatus).optional(),
    priority: z.nativeEnum(TaskPriority).optional(),
    dueDate: z.string().datetime().optional(),
    assigneeId: z.string().uuid().optional(),
    tags: z.array(z.string()).optional(),
  }),
});

export const updateTaskSchema = z.object({
  params: z.object({
    id: z.string().uuid('Invalid task ID'),
  }),
  body: z.object({
    title: z
      .string()
      .min(3, 'Title must be at least 3 characters')
      .max(200, 'Title must not exceed 200 characters')
      .optional(),
    description: z.string().max(2000, 'Description too long').optional(),
    status: z.nativeEnum(TaskStatus).optional(),
    priority: z.nativeEnum(TaskPriority).optional(),
    dueDate: z.string().datetime().optional().nullable(),
    assigneeId: z.string().uuid().optional().nullable(),
    tags: z.array(z.string()).optional(),
  }),
});

export const getTaskSchema = z.object({
  params: z.object({
    id: z.string().uuid('Invalid task ID'),
  }),
});

export const getTasksSchema = z.object({
  query: z.object({
    page: z.string().regex(/^\d+$/).transform(Number).optional(),
    limit: z.string().regex(/^\d+$/).transform(Number).optional(),
    status: z.nativeEnum(TaskStatus).optional(),
    priority: z.nativeEnum(TaskPriority).optional(),
    assigneeId: z.string().uuid().optional(),
    ownerId: z.string().uuid().optional(),
    search: z.string().optional(),
    sortBy: z.string().optional(),
    sortOrder: z.enum(['asc', 'desc']).optional(),
  }),
});

export const deleteTaskSchema = z.object({
  params: z.object({
    id: z.string().uuid('Invalid task ID'),
  }),
});
```

### 7.2 Validators para Users

Crear `src/validators/user.validator.ts`:

```typescript
import { z } from 'zod';
import { UserRole } from '@prisma/client';

export const getUserSchema = z.object({
  params: z.object({
    id: z.string().uuid('Invalid user ID'),
  }),
});

export const updateUserSchema = z.object({
  params: z.object({
    id: z.string().uuid('Invalid user ID'),
  }),
  body: z.object({
    email: z.string().email('Invalid email format').optional(),
    username: z
      .string()
      .min(3, 'Username must be at least 3 characters')
      .max(30, 'Username must not exceed 30 characters')
      .regex(/^[a-zA-Z0-9_]+$/, 'Username can only contain letters, numbers, and underscores')
      .optional(),
    firstName: z.string().max(50).optional(),
    lastName: z.string().max(50).optional(),
    role: z.nativeEnum(UserRole).optional(),
    isActive: z.boolean().optional(),
  }),
});

export const getUsersSchema = z.object({
  query: z.object({
    page: z.string().regex(/^\d+$/).transform(Number).optional(),
    limit: z.string().regex(/^\d+$/).transform(Number).optional(),
    role: z.nativeEnum(UserRole).optional(),
    isActive: z.string().transform((val) => val === 'true').optional(),
    search: z.string().optional(),
    sortBy: z.string().optional(),
    sortOrder: z.enum(['asc', 'desc']).optional(),
  }),
});

export const deleteUserSchema = z.object({
  params: z.object({
    id: z.string().uuid('Invalid user ID'),
  }),
});
```

---


## 8. Implementación de Caché con Redis

### 8.1 Configurar Redis

Crear `src/config/redis.ts`:

```typescript
import Redis from 'ioredis';
import logger from './logger';

class RedisClient {
  private static instance: Redis;

  private constructor() {}

  public static getInstance(): Redis {
    if (!RedisClient.instance) {
      RedisClient.instance = new Redis({
        host: process.env.REDIS_HOST || 'localhost',
        port: parseInt(process.env.REDIS_PORT || '6379'),
        password: process.env.REDIS_PASSWORD || undefined,
        retryStrategy: (times) => {
          const delay = Math.min(times * 50, 2000);
          return delay;
        },
        maxRetriesPerRequest: 3,
      });

      RedisClient.instance.on('connect', () => {
        logger.info('Redis connected successfully');
      });

      RedisClient.instance.on('error', (error) => {
        logger.error('Redis connection error:', error);
      });

      RedisClient.instance.on('close', () => {
        logger.warn('Redis connection closed');
      });
    }

    return RedisClient.instance;
  }

  public static async disconnect(): Promise<void> {
    if (RedisClient.instance) {
      await RedisClient.instance.quit();
    }
  }
}

export const redis = RedisClient.getInstance();
export default RedisClient;
```

### 8.2 Cache Service

Crear `src/services/cache.service.ts`:

```typescript
import { redis } from '../config/redis';
import logger from '../config/logger';

export class CacheService {
  private readonly DEFAULT_TTL = 3600; // 1 hora en segundos

  async get<T>(key: string): Promise<T | null> {
    try {
      const data = await redis.get(key);
      if (!data) return null;
      return JSON.parse(data) as T;
    } catch (error) {
      logger.error('Cache get error:', error);
      return null;
    }
  }

  async set(key: string, value: any, ttl: number = this.DEFAULT_TTL): Promise<void> {
    try {
      await redis.setex(key, ttl, JSON.stringify(value));
    } catch (error) {
      logger.error('Cache set error:', error);
    }
  }

  async delete(key: string): Promise<void> {
    try {
      await redis.del(key);
    } catch (error) {
      logger.error('Cache delete error:', error);
    }
  }

  async deletePattern(pattern: string): Promise<void> {
    try {
      const keys = await redis.keys(pattern);
      if (keys.length > 0) {
        await redis.del(...keys);
      }
    } catch (error) {
      logger.error('Cache delete pattern error:', error);
    }
  }

  async exists(key: string): Promise<boolean> {
    try {
      const result = await redis.exists(key);
      return result === 1;
    } catch (error) {
      logger.error('Cache exists error:', error);
      return false;
    }
  }

  async increment(key: string, ttl?: number): Promise<number> {
    try {
      const value = await redis.incr(key);
      if (ttl) {
        await redis.expire(key, ttl);
      }
      return value;
    } catch (error) {
      logger.error('Cache increment error:', error);
      return 0;
    }
  }

  // Cache con función de fallback
  async getOrSet<T>(
    key: string,
    fetchFunction: () => Promise<T>,
    ttl: number = this.DEFAULT_TTL
  ): Promise<T> {
    // Intentar obtener del cache
    const cached = await this.get<T>(key);
    if (cached !== null) {
      logger.debug(`Cache hit: ${key}`);
      return cached;
    }

    // Si no está en cache, ejecutar función y guardar
    logger.debug(`Cache miss: ${key}`);
    const data = await fetchFunction();
    await this.set(key, data, ttl);
    return data;
  }

  // Generar claves de cache consistentes
  generateKey(prefix: string, ...parts: (string | number)[]): string {
    return `${prefix}:${parts.join(':')}`;
  }
}
```

### 8.3 Cache Middleware

Crear `src/middlewares/cache.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import { CacheService } from '../services/cache.service';
import logger from '../config/logger';

const cacheService = new CacheService();

export const cacheMiddleware = (ttl: number = 300) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    // Solo cachear GET requests
    if (req.method !== 'GET') {
      return next();
    }

    // Generar clave de cache basada en URL y query params
    const cacheKey = cacheService.generateKey(
      'route',
      req.originalUrl,
      req.user?.userId || 'anonymous'
    );

    try {
      // Intentar obtener del cache
      const cachedData = await cacheService.get(cacheKey);
      
      if (cachedData) {
        logger.debug(`Serving from cache: ${cacheKey}`);
        return res.json(cachedData);
      }

      // Si no está en cache, interceptar res.json para guardar
      const originalJson = res.json.bind(res);
      res.json = function (data: any) {
        // Guardar en cache solo si es exitoso
        if (res.statusCode >= 200 && res.statusCode < 300) {
          cacheService.set(cacheKey, data, ttl).catch((err) => {
            logger.error('Error saving to cache:', err);
          });
        }
        return originalJson(data);
      };

      next();
    } catch (error) {
      logger.error('Cache middleware error:', error);
      next();
    }
  };
};

// Middleware para invalidar cache
export const invalidateCache = (patterns: string[]) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    // Ejecutar después de que la respuesta se envíe
    res.on('finish', async () => {
      if (res.statusCode >= 200 && res.statusCode < 300) {
        for (const pattern of patterns) {
          await cacheService.deletePattern(pattern);
          logger.debug(`Cache invalidated: ${pattern}`);
        }
      }
    });
    next();
  };
};
```

### 8.4 Integrar Cache en Task Service

Actualizar `src/services/task.service.ts` (ejemplo parcial):

```typescript
import { TaskRepository } from '../repositories/task.repository';
import { CacheService } from './cache.service';
import { CreateTaskDTO, UpdateTaskDTO, TaskFilterDTO } from '../models/task.model';
import { PaginationParams } from '../models/common.model';

export class TaskService {
  private taskRepository: TaskRepository;
  private cacheService: CacheService;

  constructor() {
    this.taskRepository = new TaskRepository();
    this.cacheService = new CacheService();
  }

  async getTaskById(id: string, userId: string) {
    const cacheKey = this.cacheService.generateKey('task', id);
    
    return this.cacheService.getOrSet(
      cacheKey,
      async () => {
        const task = await this.taskRepository.findById(id);
        if (!task) {
          throw new NotFoundError('Task');
        }
        // Verificar permisos...
        return task;
      },
      300 // 5 minutos
    );
  }

  async createTask(data: CreateTaskDTO, userId: string) {
    const task = await this.taskRepository.create({
      ...data,
      ownerId: userId,
    });

    // Invalidar cache de listas
    await this.cacheService.deletePattern('tasks:list:*');
    
    return task;
  }

  async updateTask(id: string, data: UpdateTaskDTO, userId: string) {
    // ... lógica de actualización ...
    
    // Invalidar cache específico y listas
    await this.cacheService.delete(this.cacheService.generateKey('task', id));
    await this.cacheService.deletePattern('tasks:list:*');
    
    return updatedTask;
  }
}
```

---


## 9. Testing Completo

### 9.1 Configurar Jest

Crear `jest.config.js`:

```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.ts', '**/?(*.)+(spec|test).ts'],
  transform: {
    '^.+\\.ts$': 'ts-jest',
  },
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts',
    '!src/**/*.test.ts',
    '!src/**/*.spec.ts',
    '!src/server.ts',
  ],
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'html'],
  moduleFileExtensions: ['ts', 'js', 'json'],
  setupFilesAfterEnv: ['<rootDir>/src/tests/setup.ts'],
  testTimeout: 10000,
};
```

### 9.2 Setup de Tests

Crear `src/tests/setup.ts`:

```typescript
import { prisma } from '../config/database';
import RedisClient from '../config/redis';

// Setup antes de todos los tests
beforeAll(async () => {
  // Conectar a base de datos de test
  await prisma.$connect();
});

// Cleanup después de cada test
afterEach(async () => {
  // Limpiar base de datos
  const deleteUsers = prisma.user.deleteMany();
  const deleteTasks = prisma.task.deleteMany();
  const deleteTags = prisma.tag.deleteMany();
  const deleteRefreshTokens = prisma.refreshToken.deleteMany();

  await prisma.$transaction([
    deleteRefreshTokens,
    deleteTasks,
    deleteTags,
    deleteUsers,
  ]);
});

// Cleanup después de todos los tests
afterAll(async () => {
  await prisma.$disconnect();
  await RedisClient.disconnect();
});
```

### 9.3 Test Helpers

Crear `src/tests/helpers/testHelpers.ts`:

```typescript
import { prisma } from '../../config/database';
import { PasswordUtil } from '../../utils/password.util';
import { JWTUtil } from '../../utils/jwt.util';
import { UserRole } from '@prisma/client';

export class TestHelpers {
  static async createTestUser(overrides = {}) {
    const defaultUser = {
      email: 'test@example.com',
      username: 'testuser',
      password: await PasswordUtil.hash('Test123!@#'),
      firstName: 'Test',
      lastName: 'User',
      role: UserRole.USER,
      isActive: true,
      emailVerified: true,
    };

    return prisma.user.create({
      data: { ...defaultUser, ...overrides },
    });
  }

  static async createTestAdmin() {
    return this.createTestUser({
      email: 'admin@example.com',
      username: 'admin',
      role: UserRole.ADMIN,
    });
  }

  static generateAuthToken(user: any) {
    return JWTUtil.generateAccessToken({
      userId: user.id,
      email: user.email,
      role: user.role,
    });
  }

  static async createTestTask(userId: string, overrides = {}) {
    const defaultTask = {
      title: 'Test Task',
      description: 'Test Description',
      ownerId: userId,
    };

    return prisma.task.create({
      data: { ...defaultTask, ...overrides },
    });
  }

  static async createMultipleTasks(userId: string, count: number) {
    const tasks = [];
    for (let i = 0; i < count; i++) {
      tasks.push(
        this.createTestTask(userId, {
          title: `Test Task ${i + 1}`,
        })
      );
    }
    return Promise.all(tasks);
  }
}
```

### 9.4 Unit Tests - Password Util

Crear `src/tests/unit/utils/password.util.test.ts`:

```typescript
import { PasswordUtil } from '../../../utils/password.util';

describe('PasswordUtil', () => {
  describe('hash', () => {
    it('should hash a password', async () => {
      const password = 'Test123!@#';
      const hashed = await PasswordUtil.hash(password);

      expect(hashed).toBeDefined();
      expect(hashed).not.toBe(password);
      expect(hashed.length).toBeGreaterThan(0);
    });

    it('should generate different hashes for same password', async () => {
      const password = 'Test123!@#';
      const hash1 = await PasswordUtil.hash(password);
      const hash2 = await PasswordUtil.hash(password);

      expect(hash1).not.toBe(hash2);
    });
  });

  describe('compare', () => {
    it('should return true for correct password', async () => {
      const password = 'Test123!@#';
      const hashed = await PasswordUtil.hash(password);
      const isValid = await PasswordUtil.compare(password, hashed);

      expect(isValid).toBe(true);
    });

    it('should return false for incorrect password', async () => {
      const password = 'Test123!@#';
      const hashed = await PasswordUtil.hash(password);
      const isValid = await PasswordUtil.compare('WrongPassword', hashed);

      expect(isValid).toBe(false);
    });
  });

  describe('validate', () => {
    it('should validate a strong password', () => {
      const result = PasswordUtil.validate('Test123!@#');

      expect(result.valid).toBe(true);
      expect(result.errors).toHaveLength(0);
    });

    it('should reject password without uppercase', () => {
      const result = PasswordUtil.validate('test123!@#');

      expect(result.valid).toBe(false);
      expect(result.errors).toContain('Password must contain at least one uppercase letter');
    });

    it('should reject password without lowercase', () => {
      const result = PasswordUtil.validate('TEST123!@#');

      expect(result.valid).toBe(false);
      expect(result.errors).toContain('Password must contain at least one lowercase letter');
    });

    it('should reject password without number', () => {
      const result = PasswordUtil.validate('TestTest!@#');

      expect(result.valid).toBe(false);
      expect(result.errors).toContain('Password must contain at least one number');
    });

    it('should reject password without special character', () => {
      const result = PasswordUtil.validate('Test12345');

      expect(result.valid).toBe(false);
      expect(result.errors).toContain(
        'Password must contain at least one special character (!@#$%^&*)'
      );
    });

    it('should reject short password', () => {
      const result = PasswordUtil.validate('Te1!');

      expect(result.valid).toBe(false);
      expect(result.errors).toContain('Password must be at least 8 characters long');
    });
  });
});
```

### 9.5 Unit Tests - JWT Util

Crear `src/tests/unit/utils/jwt.util.test.ts`:

```typescript
import { JWTUtil } from '../../../utils/jwt.util';
import { AuthenticationError } from '../../../errors/AppError';

describe('JWTUtil', () => {
  const mockPayload = {
    userId: '123',
    email: 'test@example.com',
    role: 'USER',
  };

  describe('generateAccessToken', () => {
    it('should generate a valid access token', () => {
      const token = JWTUtil.generateAccessToken(mockPayload);

      expect(token).toBeDefined();
      expect(typeof token).toBe('string');
      expect(token.split('.')).toHaveLength(3);
    });
  });

  describe('generateRefreshToken', () => {
    it('should generate a valid refresh token', () => {
      const token = JWTUtil.generateRefreshToken(mockPayload);

      expect(token).toBeDefined();
      expect(typeof token).toBe('string');
      expect(token.split('.')).toHaveLength(3);
    });
  });

  describe('verifyAccessToken', () => {
    it('should verify a valid access token', () => {
      const token = JWTUtil.generateAccessToken(mockPayload);
      const decoded = JWTUtil.verifyAccessToken(token);

      expect(decoded.userId).toBe(mockPayload.userId);
      expect(decoded.email).toBe(mockPayload.email);
      expect(decoded.role).toBe(mockPayload.role);
    });

    it('should throw error for invalid token', () => {
      expect(() => {
        JWTUtil.verifyAccessToken('invalid-token');
      }).toThrow(AuthenticationError);
    });
  });

  describe('decodeToken', () => {
    it('should decode a token without verification', () => {
      const token = JWTUtil.generateAccessToken(mockPayload);
      const decoded = JWTUtil.decodeToken(token);

      expect(decoded).toBeDefined();
      expect(decoded?.userId).toBe(mockPayload.userId);
    });

    it('should return null for invalid token', () => {
      const decoded = JWTUtil.decodeToken('invalid-token');
      expect(decoded).toBeNull();
    });
  });
});
```

---


### 9.6 Integration Tests - Auth

Crear `src/tests/integration/auth.test.ts`:

```typescript
import request from 'supertest';
import app from '../../app';
import { TestHelpers } from '../helpers/testHelpers';

describe('Auth Integration Tests', () => {
  describe('POST /api/v1/auth/register', () => {
    it('should register a new user successfully', async () => {
      const userData = {
        email: 'newuser@example.com',
        username: 'newuser',
        password: 'Test123!@#',
        firstName: 'New',
        lastName: 'User',
      };

      const response = await request(app)
        .post('/api/v1/auth/register')
        .send(userData)
        .expect(201);

      expect(response.body.success).toBe(true);
      expect(response.body.data.user.email).toBe(userData.email);
      expect(response.body.data.user.username).toBe(userData.username);
      expect(response.body.data.accessToken).toBeDefined();
      expect(response.body.data.refreshToken).toBeDefined();
      expect(response.body.data.user.password).toBeUndefined();
    });

    it('should reject registration with existing email', async () => {
      await TestHelpers.createTestUser();

      const response = await request(app)
        .post('/api/v1/auth/register')
        .send({
          email: 'test@example.com',
          username: 'differentuser',
          password: 'Test123!@#',
        })
        .expect(409);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain('Email already registered');
    });

    it('should reject registration with weak password', async () => {
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send({
          email: 'test@example.com',
          username: 'testuser',
          password: 'weak',
        })
        .expect(400);

      expect(response.body.success).toBe(false);
    });

    it('should reject registration with invalid email', async () => {
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send({
          email: 'invalid-email',
          username: 'testuser',
          password: 'Test123!@#',
        })
        .expect(400);

      expect(response.body.success).toBe(false);
    });
  });

  describe('POST /api/v1/auth/login', () => {
    it('should login successfully with correct credentials', async () => {
      const user = await TestHelpers.createTestUser();

      const response = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'test@example.com',
          password: 'Test123!@#',
        })
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.user.id).toBe(user.id);
      expect(response.body.data.accessToken).toBeDefined();
      expect(response.body.data.refreshToken).toBeDefined();
    });

    it('should reject login with incorrect password', async () => {
      await TestHelpers.createTestUser();

      const response = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'test@example.com',
          password: 'WrongPassword123!',
        })
        .expect(401);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain('Invalid credentials');
    });

    it('should reject login with non-existent email', async () => {
      const response = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'nonexistent@example.com',
          password: 'Test123!@#',
        })
        .expect(401);

      expect(response.body.success).toBe(false);
    });
  });

  describe('GET /api/v1/auth/me', () => {
    it('should return current user profile', async () => {
      const user = await TestHelpers.createTestUser();
      const token = TestHelpers.generateAuthToken(user);

      const response = await request(app)
        .get('/api/v1/auth/me')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.userId).toBe(user.id);
      expect(response.body.data.email).toBe(user.email);
    });

    it('should reject request without token', async () => {
      const response = await request(app)
        .get('/api/v1/auth/me')
        .expect(401);

      expect(response.body.success).toBe(false);
    });

    it('should reject request with invalid token', async () => {
      const response = await request(app)
        .get('/api/v1/auth/me')
        .set('Authorization', 'Bearer invalid-token')
        .expect(401);

      expect(response.body.success).toBe(false);
    });
  });

  describe('POST /api/v1/auth/logout', () => {
    it('should logout successfully', async () => {
      const user = await TestHelpers.createTestUser();
      const token = TestHelpers.generateAuthToken(user);

      const loginResponse = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'test@example.com',
          password: 'Test123!@#',
        });

      const refreshToken = loginResponse.body.data.refreshToken;

      const response = await request(app)
        .post('/api/v1/auth/logout')
        .set('Authorization', `Bearer ${token}`)
        .send({ refreshToken })
        .expect(200);

      expect(response.body.success).toBe(true);
    });
  });
});
```

### 9.7 Integration Tests - Tasks

Crear `src/tests/integration/tasks.test.ts`:

```typescript
import request from 'supertest';
import app from '../../app';
import { TestHelpers } from '../helpers/testHelpers';
import { TaskStatus, TaskPriority } from '@prisma/client';

describe('Tasks Integration Tests', () => {
  let user: any;
  let token: string;

  beforeEach(async () => {
    user = await TestHelpers.createTestUser();
    token = TestHelpers.generateAuthToken(user);
  });

  describe('POST /api/v1/tasks', () => {
    it('should create a new task', async () => {
      const taskData = {
        title: 'New Task',
        description: 'Task description',
        priority: TaskPriority.HIGH,
      };

      const response = await request(app)
        .post('/api/v1/tasks')
        .set('Authorization', `Bearer ${token}`)
        .send(taskData)
        .expect(201);

      expect(response.body.success).toBe(true);
      expect(response.body.data.title).toBe(taskData.title);
      expect(response.body.data.ownerId).toBe(user.id);
      expect(response.body.data.status).toBe(TaskStatus.TODO);
    });

    it('should reject task creation without authentication', async () => {
      const response = await request(app)
        .post('/api/v1/tasks')
        .send({ title: 'New Task' })
        .expect(401);

      expect(response.body.success).toBe(false);
    });

    it('should reject task with invalid data', async () => {
      const response = await request(app)
        .post('/api/v1/tasks')
        .set('Authorization', `Bearer ${token}`)
        .send({ title: 'AB' }) // Too short
        .expect(400);

      expect(response.body.success).toBe(false);
    });
  });

  describe('GET /api/v1/tasks', () => {
    it('should get paginated tasks', async () => {
      await TestHelpers.createMultipleTasks(user.id, 5);

      const response = await request(app)
        .get('/api/v1/tasks?page=1&limit=3')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.data).toHaveLength(3);
      expect(response.body.data.pagination.total).toBe(5);
      expect(response.body.data.pagination.totalPages).toBe(2);
    });

    it('should filter tasks by status', async () => {
      await TestHelpers.createTestTask(user.id, { status: TaskStatus.TODO });
      await TestHelpers.createTestTask(user.id, { status: TaskStatus.DONE });

      const response = await request(app)
        .get(`/api/v1/tasks?status=${TaskStatus.TODO}`)
        .set('Authorization', `Bearer ${token}`)
        .expect(200);

      expect(response.body.data.data).toHaveLength(1);
      expect(response.body.data.data[0].status).toBe(TaskStatus.TODO);
    });
  });

  describe('GET /api/v1/tasks/:id', () => {
    it('should get a task by id', async () => {
      const task = await TestHelpers.createTestTask(user.id);

      const response = await request(app)
        .get(`/api/v1/tasks/${task.id}`)
        .set('Authorization', `Bearer ${token}`)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.id).toBe(task.id);
      expect(response.body.data.title).toBe(task.title);
    });

    it('should return 404 for non-existent task', async () => {
      const fakeId = '123e4567-e89b-12d3-a456-426614174000';

      const response = await request(app)
        .get(`/api/v1/tasks/${fakeId}`)
        .set('Authorization', `Bearer ${token}`)
        .expect(404);

      expect(response.body.success).toBe(false);
    });
  });

  describe('PATCH /api/v1/tasks/:id', () => {
    it('should update a task', async () => {
      const task = await TestHelpers.createTestTask(user.id);

      const updateData = {
        title: 'Updated Title',
        status: TaskStatus.IN_PROGRESS,
      };

      const response = await request(app)
        .patch(`/api/v1/tasks/${task.id}`)
        .set('Authorization', `Bearer ${token}`)
        .send(updateData)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.title).toBe(updateData.title);
      expect(response.body.data.status).toBe(updateData.status);
    });

    it('should not allow updating other users tasks', async () => {
      const otherUser = await TestHelpers.createTestUser({
        email: 'other@example.com',
        username: 'otheruser',
      });
      const task = await TestHelpers.createTestTask(otherUser.id);

      const response = await request(app)
        .patch(`/api/v1/tasks/${task.id}`)
        .set('Authorization', `Bearer ${token}`)
        .send({ title: 'Hacked' })
        .expect(403);

      expect(response.body.success).toBe(false);
    });
  });

  describe('DELETE /api/v1/tasks/:id', () => {
    it('should delete a task', async () => {
      const task = await TestHelpers.createTestTask(user.id);

      const response = await request(app)
        .delete(`/api/v1/tasks/${task.id}`)
        .set('Authorization', `Bearer ${token}`)
        .expect(200);

      expect(response.body.success).toBe(true);

      // Verify task is deleted
      const getResponse = await request(app)
        .get(`/api/v1/tasks/${task.id}`)
        .set('Authorization', `Bearer ${token}`)
        .expect(404);
    });
  });
});
```

---


## 10. Seguridad y Mejores Prácticas

### 10.1 Configuración de Seguridad en app.ts

Crear `src/app.ts`:

```typescript
import express, { Application } from 'express';
import helmet from 'helmet';
import cors from 'cors';
import compression from 'compression';
import { errorHandler, notFoundHandler } from './middlewares/error.middleware';
import { requestLogger } from './middlewares/requestLogger.middleware';
import { generalLimiter } from './middlewares/rateLimit.middleware';
import routes from './routes';

const app: Application = express();

// Security Middlewares
app.use(helmet()); // Protección de headers HTTP
app.use(
  cors({
    origin: process.env.CORS_ORIGIN || '*',
    credentials: true,
    methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization'],
  })
);

// Body parsing
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true, limit: '10mb' }));

// Compression
app.use(compression());

// Request logging
app.use(requestLogger);

// Rate limiting
app.use('/api', generalLimiter);

// Health check
app.get('/health', (req, res) => {
  res.status(200).json({
    status: 'OK',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
  });
});

// API Routes
app.use('/api/v1', routes);

// Error handlers
app.use(notFoundHandler);
app.use(errorHandler);

export default app;
```

### 10.2 Configuración de Routes

Crear `src/routes/index.ts`:

```typescript
import { Router } from 'express';
import authRoutes from './auth.routes';
import taskRoutes from './task.routes';
import userRoutes from './user.routes';

const router = Router();

router.use('/auth', authRoutes);
router.use('/tasks', taskRoutes);
router.use('/users', userRoutes);

export default router;
```

### 10.3 Server Entry Point

Crear `src/server.ts`:

```typescript
import app from './app';
import Database from './config/database';
import RedisClient from './config/redis';
import logger from './config/logger';
import { setupGlobalErrorHandlers } from './middlewares/error.middleware';

const PORT = process.env.PORT || 3000;

async function startServer() {
  try {
    // Conectar a base de datos
    await Database.connect();

    // Verificar conexión a Redis
    const redis = RedisClient.getInstance();
    await redis.ping();
    logger.info('Redis connection verified');

    // Setup global error handlers
    setupGlobalErrorHandlers();

    // Iniciar servidor
    const server = app.listen(PORT, () => {
      logger.info(`Server running on port ${PORT}`);
      logger.info(`Environment: ${process.env.NODE_ENV}`);
      logger.info(`Health check: http://localhost:${PORT}/health`);
    });

    // Graceful shutdown
    const gracefulShutdown = async (signal: string) => {
      logger.info(`${signal} received. Starting graceful shutdown...`);

      server.close(async () => {
        logger.info('HTTP server closed');

        try {
          await Database.disconnect();
          await RedisClient.disconnect();
          logger.info('Database and Redis connections closed');
          process.exit(0);
        } catch (error) {
          logger.error('Error during shutdown:', error);
          process.exit(1);
        }
      });

      // Force shutdown after 10 seconds
      setTimeout(() => {
        logger.error('Forced shutdown after timeout');
        process.exit(1);
      }, 10000);
    };

    process.on('SIGTERM', () => gracefulShutdown('SIGTERM'));
    process.on('SIGINT', () => gracefulShutdown('SIGINT'));
  } catch (error) {
    logger.error('Failed to start server:', error);
    process.exit(1);
  }
}

startServer();
```

### 10.4 Mejores Prácticas de Seguridad

#### Checklist de Seguridad:

**1. Autenticación y Autorización:**
- ✅ JWT con expiración corta (15 minutos)
- ✅ Refresh tokens con rotación
- ✅ Passwords hasheados con bcrypt (12 rounds)
- ✅ Validación de fortaleza de contraseñas
- ✅ Rate limiting en endpoints de auth

**2. Validación de Datos:**
- ✅ Validación con Zod en todos los endpoints
- ✅ Sanitización de inputs
- ✅ Validación de tipos con TypeScript
- ✅ Límites en tamaño de payloads

**3. Headers de Seguridad (Helmet):**
- ✅ Content-Security-Policy
- ✅ X-Frame-Options
- ✅ X-Content-Type-Options
- ✅ Strict-Transport-Security
- ✅ X-XSS-Protection

**4. Rate Limiting:**
- ✅ Límite general de requests
- ✅ Límite estricto en autenticación
- ✅ Límite en creación de recursos

**5. Logging y Monitoreo:**
- ✅ Logs estructurados con Winston
- ✅ No loggear información sensible
- ✅ Tracking de requests sospechosos
- ✅ Alertas de errores críticos

**6. Base de Datos:**
- ✅ Uso de ORM (Prisma) previene SQL injection
- ✅ Validación de UUIDs
- ✅ Índices en campos frecuentemente consultados
- ✅ Transacciones para operaciones críticas

**7. Variables de Entorno:**
- ✅ Nunca commitear .env
- ✅ Usar secretos fuertes en producción
- ✅ Diferentes configs por ambiente

**8. CORS:**
- ✅ Configurar origins permitidos
- ✅ Limitar métodos HTTP
- ✅ Configurar headers permitidos

### 10.5 Ejemplo de Middleware de Ownership

Crear `src/middlewares/ownership.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import { prisma } from '../config/database';
import { AuthorizationError, NotFoundError } from '../errors/AppError';
import { UserRole } from '@prisma/client';

export const checkTaskOwnership = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const taskId = req.params.id;
    const userId = req.user!.userId;
    const userRole = req.user!.role;

    // Admins pueden acceder a todo
    if (userRole === UserRole.ADMIN) {
      return next();
    }

    const task = await prisma.task.findUnique({
      where: { id: taskId },
      select: { ownerId: true, assigneeId: true },
    });

    if (!task) {
      throw new NotFoundError('Task');
    }

    // Verificar si es owner o assignee
    if (task.ownerId !== userId && task.assigneeId !== userId) {
      throw new AuthorizationError('You do not have access to this task');
    }

    next();
  } catch (error) {
    next(error);
  }
};
```

---


## 11. Documentación con Swagger

### 11.1 Configurar Swagger

Crear `src/docs/swagger.ts`:

```typescript
import swaggerJsdoc from 'swagger-jsdoc';
import swaggerUi from 'swagger-ui-express';
import { Application } from 'express';

const options: swaggerJsdoc.Options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Task Management API',
      version: '1.0.0',
      description: 'A comprehensive task management system API built with Node.js',
      contact: {
        name: 'API Support',
        email: 'support@taskapi.com',
      },
    },
    servers: [
      {
        url: 'http://localhost:3000/api/v1',
        description: 'Development server',
      },
      {
        url: 'https://api.taskmanagement.com/api/v1',
        description: 'Production server',
      },
    ],
    components: {
      securitySchemes: {
        bearerAuth: {
          type: 'http',
          scheme: 'bearer',
          bearerFormat: 'JWT',
        },
      },
      schemas: {
        Error: {
          type: 'object',
          properties: {
            success: {
              type: 'boolean',
              example: false,
            },
            error: {
              type: 'string',
              example: 'Error message',
            },
          },
        },
        User: {
          type: 'object',
          properties: {
            id: {
              type: 'string',
              format: 'uuid',
            },
            email: {
              type: 'string',
              format: 'email',
            },
            username: {
              type: 'string',
            },
            firstName: {
              type: 'string',
            },
            lastName: {
              type: 'string',
            },
            role: {
              type: 'string',
              enum: ['USER', 'ADMIN', 'MANAGER'],
            },
            isActive: {
              type: 'boolean',
            },
            createdAt: {
              type: 'string',
              format: 'date-time',
            },
          },
        },
        Task: {
          type: 'object',
          properties: {
            id: {
              type: 'string',
              format: 'uuid',
            },
            title: {
              type: 'string',
            },
            description: {
              type: 'string',
            },
            status: {
              type: 'string',
              enum: ['TODO', 'IN_PROGRESS', 'IN_REVIEW', 'DONE', 'CANCELLED'],
            },
            priority: {
              type: 'string',
              enum: ['LOW', 'MEDIUM', 'HIGH', 'URGENT'],
            },
            dueDate: {
              type: 'string',
              format: 'date-time',
            },
            ownerId: {
              type: 'string',
              format: 'uuid',
            },
            assigneeId: {
              type: 'string',
              format: 'uuid',
            },
            createdAt: {
              type: 'string',
              format: 'date-time',
            },
            updatedAt: {
              type: 'string',
              format: 'date-time',
            },
          },
        },
      },
    },
    security: [
      {
        bearerAuth: [],
      },
    ],
  },
  apis: ['./src/routes/*.ts', './src/controllers/*.ts'],
};

const swaggerSpec = swaggerJsdoc(options);

export const setupSwagger = (app: Application) => {
  app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
  app.get('/api-docs.json', (req, res) => {
    res.setHeader('Content-Type', 'application/json');
    res.send(swaggerSpec);
  });
};
```

### 11.2 Documentar Endpoints con JSDoc

Actualizar `src/routes/auth.routes.ts` con documentación:

```typescript
/**
 * @swagger
 * /auth/register:
 *   post:
 *     summary: Register a new user
 *     tags: [Authentication]
 *     security: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required:
 *               - email
 *               - username
 *               - password
 *             properties:
 *               email:
 *                 type: string
 *                 format: email
 *                 example: user@example.com
 *               username:
 *                 type: string
 *                 example: johndoe
 *               password:
 *                 type: string
 *                 format: password
 *                 example: Test123!@#
 *               firstName:
 *                 type: string
 *                 example: John
 *               lastName:
 *                 type: string
 *                 example: Doe
 *     responses:
 *       201:
 *         description: User registered successfully
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 success:
 *                   type: boolean
 *                   example: true
 *                 message:
 *                   type: string
 *                   example: User registered successfully
 *                 data:
 *                   type: object
 *                   properties:
 *                     user:
 *                       $ref: '#/components/schemas/User'
 *                     accessToken:
 *                       type: string
 *                     refreshToken:
 *                       type: string
 *       400:
 *         description: Validation error
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/Error'
 *       409:
 *         description: User already exists
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/Error'
 */
router.post('/register', validate(registerSchema), authController.register);

/**
 * @swagger
 * /auth/login:
 *   post:
 *     summary: Login user
 *     tags: [Authentication]
 *     security: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required:
 *               - email
 *               - password
 *             properties:
 *               email:
 *                 type: string
 *                 format: email
 *               password:
 *                 type: string
 *                 format: password
 *     responses:
 *       200:
 *         description: Login successful
 *       401:
 *         description: Invalid credentials
 */
router.post('/login', validate(loginSchema), authController.login);
```

### 11.3 Integrar Swagger en app.ts

Actualizar `src/app.ts`:

```typescript
import { setupSwagger } from './docs/swagger';

// ... resto del código ...

// Swagger documentation
if (process.env.NODE_ENV !== 'production') {
  setupSwagger(app);
  logger.info('Swagger docs available at /api-docs');
}
```

---


## 12. Optimización y Performance

### 12.1 Database Query Optimization

**Técnicas de Optimización con Prisma:**

```typescript
// ❌ N+1 Problem - Malo
async function getTasksWithOwners() {
  const tasks = await prisma.task.findMany();
  for (const task of tasks) {
    task.owner = await prisma.user.findUnique({ where: { id: task.ownerId } });
  }
  return tasks;
}

// ✅ Include/Select - Bueno
async function getTasksWithOwnersOptimized() {
  return prisma.task.findMany({
    include: {
      owner: {
        select: {
          id: true,
          username: true,
          email: true,
        },
      },
    },
  });
}

// ✅ Paginación eficiente
async function getPaginatedTasks(page: number, limit: number) {
  const skip = (page - 1) * limit;
  
  const [tasks, total] = await prisma.$transaction([
    prisma.task.findMany({
      skip,
      take: limit,
      orderBy: { createdAt: 'desc' },
    }),
    prisma.task.count(),
  ]);

  return { tasks, total };
}

// ✅ Batch operations
async function createMultipleTasks(tasksData: any[]) {
  return prisma.task.createMany({
    data: tasksData,
    skipDuplicates: true,
  });
}
```

### 12.2 Índices de Base de Datos

Ya definidos en el schema de Prisma:

```prisma
model Task {
  // ...
  @@index([ownerId])
  @@index([assigneeId])
  @@index([status])
  @@index([priority])
  @@index([dueDate])
}
```

**Verificar performance de queries:**

```typescript
// Habilitar query logging en desarrollo
const prisma = new PrismaClient({
  log: [
    { level: 'query', emit: 'event' },
  ],
});

prisma.$on('query', (e) => {
  console.log('Query: ' + e.query);
  console.log('Duration: ' + e.duration + 'ms');
});
```

### 12.3 Estrategias de Caché

**1. Cache de Queries Frecuentes:**

```typescript
// Cache de usuario por ID
async getUserById(id: string) {
  const cacheKey = this.cacheService.generateKey('user', id);
  
  return this.cacheService.getOrSet(
    cacheKey,
    async () => {
      return this.userRepository.findById(id);
    },
    3600 // 1 hora
  );
}

// Cache de listas con filtros
async getTasks(filters: TaskFilterDTO, pagination: PaginationParams) {
  const cacheKey = this.cacheService.generateKey(
    'tasks',
    'list',
    JSON.stringify(filters),
    JSON.stringify(pagination)
  );
  
  return this.cacheService.getOrSet(
    cacheKey,
    async () => {
      return this.taskRepository.findAll(filters, pagination);
    },
    300 // 5 minutos
  );
}
```

**2. Cache Invalidation Patterns:**

```typescript
// Invalidar cache específico
async updateTask(id: string, data: UpdateTaskDTO) {
  const task = await this.taskRepository.update(id, data);
  
  // Invalidar cache del task específico
  await this.cacheService.delete(
    this.cacheService.generateKey('task', id)
  );
  
  // Invalidar todas las listas de tasks
  await this.cacheService.deletePattern('tasks:list:*');
  
  return task;
}
```

### 12.4 Compression y Response Optimization

```typescript
// Ya configurado en app.ts con compression middleware
import compression from 'compression';
app.use(compression());

// Limitar campos en responses
async function getUsers() {
  return prisma.user.findMany({
    select: {
      id: true,
      username: true,
      email: true,
      // No incluir password, refreshTokens, etc.
    },
  });
}
```

### 12.5 Connection Pooling

Prisma maneja connection pooling automáticamente:

```typescript
// Configurar en DATABASE_URL
// postgresql://user:password@localhost:5432/db?connection_limit=10

// O en código
const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL,
    },
  },
});
```

### 12.6 Async/Await Best Practices

```typescript
// ❌ Malo - Operaciones secuenciales innecesarias
async function getBadData(userId: string) {
  const user = await userRepository.findById(userId);
  const tasks = await taskRepository.findByUserId(userId);
  const stats = await statsRepository.getByUserId(userId);
  return { user, tasks, stats };
}

// ✅ Bueno - Operaciones paralelas
async function getGoodData(userId: string) {
  const [user, tasks, stats] = await Promise.all([
    userRepository.findById(userId),
    taskRepository.findByUserId(userId),
    statsRepository.getByUserId(userId),
  ]);
  return { user, tasks, stats };
}

// ✅ Manejo de errores en paralelo
async function getDataWithErrorHandling(userId: string) {
  const results = await Promise.allSettled([
    userRepository.findById(userId),
    taskRepository.findByUserId(userId),
    statsRepository.getByUserId(userId),
  ]);

  return {
    user: results[0].status === 'fulfilled' ? results[0].value : null,
    tasks: results[1].status === 'fulfilled' ? results[1].value : [],
    stats: results[2].status === 'fulfilled' ? results[2].value : null,
  };
}
```

### 12.7 Monitoring y Profiling

Crear `src/middlewares/performance.middleware.ts`:

```typescript
import { Request, Response, NextFunction } from 'express';
import logger from '../config/logger';

export const performanceMonitor = (req: Request, res: Response, next: NextFunction) => {
  const start = process.hrtime();

  res.on('finish', () => {
    const [seconds, nanoseconds] = process.hrtime(start);
    const duration = seconds * 1000 + nanoseconds / 1000000; // Convert to ms

    // Log slow requests (> 1 second)
    if (duration > 1000) {
      logger.warn('Slow request detected', {
        method: req.method,
        url: req.originalUrl,
        duration: `${duration.toFixed(2)}ms`,
        statusCode: res.statusCode,
      });
    }

    // Agregar header de timing
    res.setHeader('X-Response-Time', `${duration.toFixed(2)}ms`);
  });

  next();
};
```

---


## 13. Deployment y CI/CD

### 13.1 Dockerfile

Crear `Dockerfile`:

```dockerfile
# Build stage
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./
COPY prisma ./prisma/

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Generate Prisma Client
RUN npx prisma generate

# Build TypeScript
RUN npm run build

# Production stage
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./
COPY prisma ./prisma/

# Install production dependencies only
RUN npm ci --only=production

# Copy built files from builder
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules/.prisma ./node_modules/.prisma

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Change ownership
RUN chown -R nodejs:nodejs /app

USER nodejs

EXPOSE 3000

CMD ["node", "dist/server.js"]
```

### 13.2 Docker Compose

Crear `docker-compose.yml`:

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - '3000:3000'
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://postgres:password@postgres:5432/taskdb
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - postgres
      - redis
    restart: unless-stopped

  postgres:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=taskdb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - '5432:5432'
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    ports:
      - '6379:6379'
    volumes:
      - redis_data:/data
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

### 13.3 GitHub Actions CI/CD

Crear `.github/workflows/ci.yml`:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Generate Prisma Client
        run: npx prisma generate

      - name: Run migrations
        run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db
          REDIS_HOST: localhost
          REDIS_PORT: 6379
          JWT_SECRET: test-secret
          JWT_REFRESH_SECRET: test-refresh-secret

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Build application
        run: |
          npm ci
          npx prisma generate
          npm run build

      - name: Build Docker image
        run: docker build -t task-api:${{ github.sha }} .

      # Aquí puedes agregar steps para push a Docker Hub, AWS ECR, etc.
```

### 13.4 Environment Variables para Producción

Crear `.env.production.example`:

```env
# Server
NODE_ENV=production
PORT=3000
API_VERSION=v1

# Database (usar servicios gestionados)
DATABASE_URL="postgresql://user:password@db-host:5432/taskdb?schema=public&connection_limit=10"

# JWT (usar secretos fuertes)
JWT_SECRET=<generate-strong-secret-here>
JWT_REFRESH_SECRET=<generate-strong-secret-here>
JWT_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d

# Redis
REDIS_HOST=redis-host
REDIS_PORT=6379
REDIS_PASSWORD=<redis-password>

# CORS
CORS_ORIGIN=https://yourdomain.com

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# Logging
LOG_LEVEL=info
```

### 13.5 Scripts de Deployment

Crear `scripts/deploy.sh`:

```bash
#!/bin/bash

echo "🚀 Starting deployment..."

# Pull latest code
git pull origin main

# Install dependencies
npm ci

# Generate Prisma Client
npx prisma generate

# Run migrations
npx prisma migrate deploy

# Build application
npm run build

# Restart application (usando PM2)
pm2 restart task-api

echo "✅ Deployment completed!"
```

### 13.6 PM2 Configuration

Crear `ecosystem.config.js`:

```javascript
module.exports = {
  apps: [
    {
      name: 'task-api',
      script: './dist/server.js',
      instances: 'max',
      exec_mode: 'cluster',
      env: {
        NODE_ENV: 'production',
      },
      error_file: './logs/pm2-error.log',
      out_file: './logs/pm2-out.log',
      log_date_format: 'YYYY-MM-DD HH:mm:ss Z',
      merge_logs: true,
      max_memory_restart: '1G',
      autorestart: true,
      watch: false,
    },
  ],
};
```

### 13.7 Health Checks y Readiness

Actualizar `src/app.ts`:

```typescript
// Health check detallado
app.get('/health', async (req, res) => {
  const health = {
    status: 'OK',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    environment: process.env.NODE_ENV,
    version: process.env.npm_package_version,
  };

  try {
    // Check database
    await prisma.$queryRaw`SELECT 1`;
    health.database = 'connected';

    // Check Redis
    await redis.ping();
    health.redis = 'connected';

    res.status(200).json(health);
  } catch (error) {
    health.status = 'ERROR';
    health.database = 'disconnected';
    health.redis = 'disconnected';
    res.status(503).json(health);
  }
});

// Readiness probe
app.get('/ready', async (req, res) => {
  try {
    await prisma.$queryRaw`SELECT 1`;
    res.status(200).json({ ready: true });
  } catch {
    res.status(503).json({ ready: false });
  }
});

// Liveness probe
app.get('/live', (req, res) => {
  res.status(200).json({ alive: true });
});
```

---


## 📝 Conceptos Clave para Entrevistas

### 1. Arquitectura y Patrones de Diseño

**Preguntas Comunes:**
- ¿Por qué usar arquitectura en capas?
- ¿Qué es el patrón Repository?
- ¿Diferencia entre Service y Controller?

**Respuestas:**
- **Arquitectura en capas**: Separa responsabilidades, facilita testing y mantenimiento
- **Repository Pattern**: Abstrae el acceso a datos, permite cambiar ORM sin afectar lógica de negocio
- **Service vs Controller**: Controller maneja HTTP, Service contiene lógica de negocio

### 2. Autenticación y Seguridad

**Preguntas Comunes:**
- ¿Cómo funciona JWT?
- ¿Por qué usar refresh tokens?
- ¿Cómo prevenir ataques comunes?

**Respuestas:**
- **JWT**: Token firmado que contiene claims, stateless, verificable
- **Refresh Tokens**: Permiten renovar access tokens sin re-autenticación, más seguros
- **Prevención**: Rate limiting, validación de inputs, CORS, Helmet, HTTPS

### 3. Performance y Escalabilidad

**Preguntas Comunes:**
- ¿Cómo optimizar queries de base de datos?
- ¿Cuándo usar caché?
- ¿Qué es el problema N+1?

**Respuestas:**
- **Optimización**: Índices, select específicos, paginación, batch operations
- **Caché**: Para datos frecuentemente leídos y poco modificados (usuarios, configuraciones)
- **N+1**: Múltiples queries en loop, solución: usar include/joins

### 4. Testing

**Preguntas Comunes:**
- ¿Diferencia entre unit e integration tests?
- ¿Qué testear en una API?
- ¿Cómo mockear dependencias?

**Respuestas:**
- **Unit**: Funciones aisladas, rápidos, sin DB
- **Integration**: Flujos completos, con DB, más lentos
- **Qué testear**: Lógica de negocio, validaciones, autenticación, casos edge
- **Mocking**: Jest mocks, test doubles, dependency injection

### 5. Error Handling

**Preguntas Comunes:**
- ¿Cómo manejar errores en async/await?
- ¿Qué son errores operacionales vs programáticos?
- ¿Cómo loggear errores?

**Respuestas:**
- **Async/await**: Try-catch, middleware de errores, asyncHandler wrapper
- **Operacionales**: Esperados (404, validación), Programáticos: bugs (null reference)
- **Logging**: Winston, niveles (error, warn, info), no loggear info sensible

### 6. Database y ORM

**Preguntas Comunes:**
- ¿Por qué usar un ORM?
- ¿Qué son las migraciones?
- ¿Cómo manejar transacciones?

**Respuestas:**
- **ORM**: Type-safety, previene SQL injection, abstracción, migraciones
- **Migraciones**: Control de versiones de schema, reproducibles, rollback
- **Transacciones**: ACID, múltiples operaciones atómicas

### 7. Node.js Específico

**Preguntas Comunes:**
- ¿Qué es el Event Loop?
- ¿Diferencia entre process.nextTick y setImmediate?
- ¿Cómo manejar operaciones CPU-intensive?

**Respuestas:**
- **Event Loop**: Single-threaded, non-blocking I/O, fases (timers, I/O, check)
- **nextTick vs setImmediate**: nextTick ejecuta antes del siguiente tick, setImmediate en check phase
- **CPU-intensive**: Worker threads, child processes, microservicios

---

## 🎯 Ejercicios Prácticos

### Ejercicio 1: Implementar Soft Delete

Modifica el sistema para que las tareas no se eliminen físicamente:

```typescript
// Agregar campo en schema
model Task {
  // ...
  deletedAt DateTime?
}

// Middleware de Prisma para filtrar soft-deleted
prisma.$use(async (params, next) => {
  if (params.model === 'Task') {
    if (params.action === 'findMany' || params.action === 'findFirst') {
      params.args.where = { ...params.args.where, deletedAt: null };
    }
  }
  return next(params);
});

// Método de soft delete
async softDelete(id: string) {
  return prisma.task.update({
    where: { id },
    data: { deletedAt: new Date() },
  });
}
```

### Ejercicio 2: Implementar Búsqueda Full-Text

```typescript
// En task.repository.ts
async search(query: string) {
  return prisma.task.findMany({
    where: {
      OR: [
        { title: { contains: query, mode: 'insensitive' } },
        { description: { contains: query, mode: 'insensitive' } },
      ],
    },
  });
}
```

### Ejercicio 3: Implementar Notificaciones

```typescript
// Crear servicio de notificaciones
class NotificationService {
  async notifyTaskAssigned(task: Task, assignee: User) {
    // Enviar email, push notification, etc.
    logger.info(`Task ${task.id} assigned to ${assignee.email}`);
  }
}

// Usar en task.service.ts
async assignTask(taskId: string, assigneeId: string) {
  const task = await this.taskRepository.update(taskId, { assigneeId });
  const assignee = await this.userRepository.findById(assigneeId);
  
  await this.notificationService.notifyTaskAssigned(task, assignee);
  
  return task;
}
```

### Ejercicio 4: Implementar Webhooks

```typescript
// Crear webhook service
class WebhookService {
  async trigger(event: string, data: any) {
    const webhooks = await prisma.webhook.findMany({
      where: { event, isActive: true },
    });

    for (const webhook of webhooks) {
      try {
        await fetch(webhook.url, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ event, data }),
        });
      } catch (error) {
        logger.error(`Webhook failed: ${webhook.url}`, error);
      }
    }
  }
}
```

---

## 🚀 Próximos Pasos y Mejoras

### Features Avanzados para Implementar:

1. **WebSockets para Real-time Updates**
   - Socket.io para notificaciones en tiempo real
   - Actualización de estado de tareas en vivo

2. **File Upload**
   - Multer para manejo de archivos
   - S3 o similar para almacenamiento
   - Validación de tipos y tamaños

3. **Email Service**
   - Nodemailer para envío de emails
   - Templates con Handlebars
   - Queue con Bull para procesamiento asíncrono

4. **Advanced Search**
   - Elasticsearch para búsqueda full-text
   - Filtros complejos y facetas
   - Autocompletado

5. **Analytics y Reporting**
   - Dashboard de métricas
   - Exportación a PDF/Excel
   - Gráficos con Chart.js

6. **Multi-tenancy**
   - Organizaciones/Workspaces
   - Permisos por organización
   - Aislamiento de datos

7. **API Versioning**
   - Múltiples versiones de API
   - Deprecation strategy
   - Backward compatibility

8. **GraphQL API**
   - Apollo Server
   - Type-safe queries
   - Subscriptions para real-time

---

## 📚 Recursos Adicionales

### Documentación Oficial:
- [Node.js Docs](https://nodejs.org/docs/)
- [Express.js](https://expressjs.com/)
- [Prisma](https://www.prisma.io/docs)
- [TypeScript](https://www.typescriptlang.org/docs/)

### Libros Recomendados:
- "Node.js Design Patterns" - Mario Casciaro
- "Clean Code" - Robert C. Martin
- "Designing Data-Intensive Applications" - Martin Kleppmann

### Cursos y Tutoriales:
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [Prisma Tutorial](https://www.prisma.io/docs/getting-started)

### Herramientas Útiles:
- **Postman/Insomnia**: Testing de APIs
- **Prisma Studio**: GUI para base de datos
- **Docker Desktop**: Containerización local
- **VS Code Extensions**: ESLint, Prettier, Prisma

---

## 🎓 Conclusión

Este proyecto cubre los conceptos fundamentales y avanzados que necesitas dominar para trabajar profesionalmente con Node.js:

✅ **Arquitectura escalable** con separación de responsabilidades
✅ **Autenticación robusta** con JWT y refresh tokens
✅ **Validación completa** con Zod
✅ **Testing exhaustivo** con Jest
✅ **Seguridad** con Helmet, rate limiting, y mejores prácticas
✅ **Performance** con caché, optimización de queries, y monitoring
✅ **Documentación** con Swagger
✅ **CI/CD** con GitHub Actions y Docker

### Para el Curso de YouTube:

**Estructura Sugerida:**
1. **Módulo 1**: Setup y Configuración (Videos 1-2)
2. **Módulo 2**: Arquitectura y Base de Datos (Videos 3-5)
3. **Módulo 3**: Autenticación y Seguridad (Videos 6-8)
4. **Módulo 4**: CRUD y Validación (Videos 9-11)
5. **Módulo 5**: Testing (Videos 12-14)
6. **Módulo 6**: Caché y Performance (Videos 15-16)
7. **Módulo 7**: Documentación y Deployment (Videos 17-18)
8. **Módulo 8**: Conceptos Avanzados (Videos 19-20)

Cada video puede durar entre 15-30 minutos, enfocándose en implementar una funcionalidad específica mientras explicas los conceptos teóricos relevantes.

**¡Buena suerte con tu curso!** 🚀

---

## 📞 Soporte

Si tienes preguntas o encuentras problemas:
1. Revisa la documentación oficial de cada tecnología
2. Busca en Stack Overflow
3. Revisa los issues en GitHub de las librerías
4. Únete a comunidades de Node.js en Discord/Slack

**Happy Coding!** 💻✨
