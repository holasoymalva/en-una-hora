
# Introducción a Nest.js - Guía de 1 Hora

Aprenderás los conceptos fundamentales de Nest.js, un framework progresivo para construir aplicaciones backend eficientes y escalables. 🚀

## Requisitos Previos
- Node.js instalado (v12 o superior)
- Conocimientos básicos de TypeScript
- Un editor de código (VS Code recomendado)
- Familiaridad con APIs REST

## 1. Configuración Inicial

```bash
# Instalar Nest CLI globalmente
npm i -g @nestjs/cli

# Crear nuevo proyecto
nest new mi-proyecto-nest

# Entrar al directorio
cd mi-proyecto-nest

# Iniciar en modo desarrollo
npm run start:dev
```

## 2. Estructura del Proyecto

```plaintext
mi-proyecto-nest/
├── src/
│   ├── app.controller.ts
│   ├── app.service.ts
│   ├── app.module.ts
│   └── main.ts
├── test/
├── package.json
└── tsconfig.json
```

> **Nota sobre la Estructura:**
> - `main.ts`: Punto de entrada de la aplicación
> - `app.module.ts`: Módulo raíz
> - `app.controller.ts`: Controlador básico
> - `app.service.ts`: Servicios y lógica de negocio

## 3. Controladores

```typescript
// src/users/users.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.usersService.findOne(+id);
  }

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }
}

// src/users/dto/create-user.dto.ts
export class CreateUserDto {
  readonly name: string;
  readonly email: string;
  readonly age: number;
}
```

> **Nota sobre Controladores:**
> - Manejan las rutas y peticiones HTTP
> - Usan decoradores para definir endpoints
> - Pueden inyectar servicios
> - Los DTOs definen la estructura de datos

## 4. Servicios y Providers

```typescript
// src/users/users.service.ts
import { Injectable } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';

@Injectable()
export class UsersService {
  private readonly users = [];

  create(createUserDto: CreateUserDto) {
    const user = {
      id: this.users.length + 1,
      ...createUserDto,
    };
    this.users.push(user);
    return user;
  }

  findAll() {
    return this.users;
  }

  findOne(id: number) {
    return this.users.find(user => user.id === id);
  }
}
```

> **Nota sobre Servicios:**
> - Contienen la lógica de negocio
> - Son inyectables (Dependency Injection)
> - Singleton por defecto
> - Pueden inyectar otros servicios

## 5. Módulos

```typescript
// src/users/users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService], // Si queremos usar el servicio en otros módulos
})
export class UsersModule {}

// src/app.module.ts
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';

@Module({
  imports: [UsersModule],
})
export class AppModule {}
```

> **Nota sobre Módulos:**
> - Organizan la aplicación en bloques
> - Encapsulan funcionalidades relacionadas
> - Pueden importar otros módulos
> - Definen el scope de los providers

## 6. Middleware y Pipes

```typescript
// src/common/validation.pipe.ts
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { validate } from 'class-validator';
import { plainToClass } from 'class-transformer';

@Injectable()
export class ValidationPipe implements PipeTransform<any> {
  async transform(value: any, { metatype }: ArgumentMetadata) {
    if (!metatype || !this.toValidate(metatype)) {
      return value;
    }
    const object = plainToClass(metatype, value);
    const errors = await validate(object);
    if (errors.length > 0) {
      throw new BadRequestException('Validation failed');
    }
    return value;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}

// Uso en DTOs
import { IsString, IsEmail, IsNumber, Min } from 'class-validator';

export class CreateUserDto {
  @IsString()
  readonly name: string;

  @IsEmail()
  readonly email: string;

  @IsNumber()
  @Min(0)
  readonly age: number;
}

// Middleware de ejemplo
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();
  }
}
```

## Ejemplo Práctico Completo: API REST de Tareas

```typescript
// src/tasks/task.entity.ts
export class Task {
  id: number;
  title: string;
  description: string;
  completed: boolean;
}

// src/tasks/dto/create-task.dto.ts
import { IsString, IsNotEmpty } from 'class-validator';

export class CreateTaskDto {
  @IsString()
  @IsNotEmpty()
  title: string;

  @IsString()
  description: string;
}

// src/tasks/tasks.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { Task } from './task.entity';
import { CreateTaskDto } from './dto/create-task.dto';

@Injectable()
export class TasksService {
  private tasks: Task[] = [];

  getAllTasks(): Task[] {
    return this.tasks;
  }

  getTaskById(id: number): Task {
    const found = this.tasks.find(task => task.id === id);
    if (!found) {
      throw new NotFoundException(`Task with ID "${id}" not found`);
    }
    return found;
  }

  createTask(createTaskDto: CreateTaskDto): Task {
    const { title, description } = createTaskDto;
    
    const task: Task = {
      id: this.tasks.length + 1,
      title,
      description,
      completed: false,
    };

    this.tasks.push(task);
    return task;
  }

  updateTaskStatus(id: number, completed: boolean): Task {
    const task = this.getTaskById(id);
    task.completed = completed;
    return task;
  }

  deleteTask(id: number): void {
    const found = this.getTaskById(id);
    this.tasks = this.tasks.filter(task => task.id !== found.id);
  }
}

// src/tasks/tasks.controller.ts
import { Controller, Get, Post, Body, Param, Delete, Patch, UsePipes, ValidationPipe } from '@nestjs/common';
import { TasksService } from './tasks.service';
import { CreateTaskDto } from './dto/create-task.dto';
import { Task } from './task.entity';

@Controller('tasks')
export class TasksController {
  constructor(private tasksService: TasksService) {}

  @Get()
  getAllTasks(): Task[] {
    return this.tasksService.getAllTasks();
  }

  @Get('/:id')
  getTaskById(@Param('id') id: number): Task {
    return this.tasksService.getTaskById(id);
  }

  @Post()
  @UsePipes(ValidationPipe)
  createTask(@Body() createTaskDto: CreateTaskDto): Task {
    return this.tasksService.createTask(createTaskDto);
  }

  @Patch('/:id/status')
  updateTaskStatus(
    @Param('id') id: number,
    @Body('completed') completed: boolean,
  ): Task {
    return this.tasksService.updateTaskStatus(id, completed);
  }

  @Delete('/:id')
  deleteTask(@Param('id') id: number): void {
    this.tasksService.deleteTask(id);
  }
}
```

## Recursos Adicionales
- [Documentación oficial de Nest.js](https://docs.nestjs.com/)
- [Repositorio de ejemplos oficiales](https://github.com/nestjs/nest/tree/master/sample)
- [Curso gratuito en YouTube](https://www.youtube.com/watch?v=F_oOtaxb0L8)
- [Awesome Nest.js](https://github.com/juliandavidmr/awesome-nestjs)

## Mejores Prácticas

1. **Estructura del Proyecto**
   - Un módulo por funcionalidad
   - Separar DTOs y entidades
   - Usar interfaces y tipos

2. **Validación**
   - Usar class-validator
   - Implementar pipes de validación
   - Manejar errores globalmente

3. **Rendimiento**
   - Usar interceptores para cache
   - Implementar paginación
   - Optimizar consultas a BD

4. **Seguridad**
   - Implementar autenticación
   - Usar CORS
   - Validar entrada de datos

¡Felicitaciones! 🎉 Has completado la introducción a Nest.js. Ahora tienes las bases necesarias para construir APIs robustas y escalables.
