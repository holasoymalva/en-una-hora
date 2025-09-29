# 📚 Curso Práctico de SQL

## Introducción

Bienvenido al curso práctico de SQL. Este curso está diseñado para aprender SQL de manera práctica, construyendo proyectos reales desde cero. No solo veremos teoría, sino que aplicaremos cada concepto en mini proyectos que te permitirán dominar las bases de datos.

**¿Qué aprenderás?**
- Fundamentos de bases de datos relacionales
- Crear y gestionar bases de datos
- Realizar consultas complejas
- Trabajar con múltiples tablas
- Optimizar consultas
- Proyectos reales aplicando todos los conceptos

---

## 🎯 Proyecto 1: Sistema de Biblioteca Personal

En este primer proyecto crearemos una base de datos para gestionar una biblioteca personal. Es perfecto para comenzar porque involucra conceptos fundamentales de SQL.

### 1.1 Creación de la Base de Datos

**Concepto:** Una base de datos es un contenedor que almacena información organizada en tablas.

```sql
-- Crear la base de datos
CREATE DATABASE biblioteca_personal;

-- Seleccionar la base de datos para trabajar con ella
USE biblioteca_personal;
```

**📝 Nota:** 
- `CREATE DATABASE` crea una nueva base de datos
- `USE` selecciona la base de datos activa
- Los comentarios en SQL comienzan con `--`

### 1.2 Crear Nuestra Primera Tabla: Libros

**Concepto:** Las tablas son estructuras que organizan datos en filas y columnas. Cada columna tiene un tipo de dato específico.

```sql
CREATE TABLE libros (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(200) NOT NULL,
    autor VARCHAR(100) NOT NULL,
    año_publicacion INT,
    genero VARCHAR(50),
    numero_paginas INT,
    precio DECIMAL(10, 2),
    fecha_adquisicion DATE,
    leido BOOLEAN DEFAULT FALSE
);
```

**📝 Notas importantes:**
- `INT`: Número entero
- `VARCHAR(n)`: Texto de longitud variable (máximo n caracteres)
- `DECIMAL(10,2)`: Número decimal (10 dígitos totales, 2 decimales)
- `DATE`: Fecha en formato YYYY-MM-DD
- `BOOLEAN`: Verdadero o falso
- `PRIMARY KEY`: Identificador único de cada registro
- `AUTO_INCREMENT`: El valor se genera automáticamente
- `NOT NULL`: Campo obligatorio
- `DEFAULT`: Valor por defecto si no se especifica

### 1.3 Insertar Datos (INSERT)

**Concepto:** INSERT permite agregar nuevos registros a una tabla.

```sql
-- Insertar un libro especificando todas las columnas
INSERT INTO libros (titulo, autor, año_publicacion, genero, numero_paginas, precio, fecha_adquisicion, leido)
VALUES ('Cien Años de Soledad', 'Gabriel García Márquez', 1967, 'Realismo Mágico', 471, 299.99, '2024-01-15', TRUE);

-- Insertar sin especificar el ID (se genera automáticamente)
INSERT INTO libros (titulo, autor, año_publicacion, genero, numero_paginas, precio, fecha_adquisicion)
VALUES ('1984', 'George Orwell', 1949, 'Distopía', 328, 249.50, '2024-02-10');

-- Insertar múltiples registros a la vez
INSERT INTO libros (titulo, autor, año_publicacion, genero, numero_paginas, precio, fecha_adquisicion, leido)
VALUES 
    ('El Principito', 'Antoine de Saint-Exupéry', 1943, 'Fábula', 96, 150.00, '2024-01-20', TRUE),
    ('Don Quijote de la Mancha', 'Miguel de Cervantes', 1605, 'Novela', 863, 399.99, '2024-03-05', FALSE),
    ('Orgullo y Prejuicio', 'Jane Austen', 1813, 'Romance', 432, 279.99, '2024-02-28', TRUE);
```

**📝 Nota práctica:**
- Si omites una columna con `DEFAULT`, se usará el valor por defecto
- Puedes insertar múltiples filas separándolas con comas
- Las fechas se escriben entre comillas simples: 'YYYY-MM-DD'
- Los textos van entre comillas simples

### 1.4 Consultas Básicas (SELECT)

**Concepto:** SELECT permite recuperar información de la base de datos.

```sql
-- Seleccionar todos los campos de todos los libros
SELECT * FROM libros;

-- Seleccionar solo algunos campos
SELECT titulo, autor, precio FROM libros;

-- Seleccionar con alias (renombrar columnas en el resultado)
SELECT 
    titulo AS 'Título del Libro',
    autor AS 'Escritor',
    precio AS 'Precio en MXN'
FROM libros;
```

**📝 Nota:**
- `*` significa "todas las columnas"
- `AS` crea un alias (nombre alternativo) para la columna
- Los alias mejoran la legibilidad de los resultados

### 1.5 Filtrar Datos (WHERE)

**Concepto:** WHERE filtra los registros según condiciones específicas.

```sql
-- Libros que ya hemos leído
SELECT titulo, autor FROM libros
WHERE leido = TRUE;

-- Libros publicados después del año 1900
SELECT titulo, autor, año_publicacion FROM libros
WHERE año_publicacion > 1900;

-- Libros de un género específico
SELECT titulo, autor, genero FROM libros
WHERE genero = 'Realismo Mágico';

-- Libros con precio menor a 300
SELECT titulo, precio FROM libros
WHERE precio < 300;

-- Combinar condiciones con AND
SELECT titulo, autor, precio FROM libros
WHERE precio < 300 AND leido = TRUE;

-- Combinar condiciones con OR
SELECT titulo, genero FROM libros
WHERE genero = 'Distopía' OR genero = 'Romance';
```

**📝 Operadores de comparación:**
- `=` igual
- `<>` o `!=` diferente
- `>` mayor que
- `<` menor que
- `>=` mayor o igual
- `<=` menor o igual
- `AND` ambas condiciones deben cumplirse
- `OR` al menos una condición debe cumplirse

### 1.6 Búsquedas con LIKE

**Concepto:** LIKE permite buscar patrones en textos.

```sql
-- Libros cuyo título comienza con "El"
SELECT titulo, autor FROM libros
WHERE titulo LIKE 'El%';

-- Libros que contienen la palabra "amor" en cualquier parte del título
SELECT titulo FROM libros
WHERE titulo LIKE '%amor%';

-- Autores cuyo apellido termina en "ez"
SELECT DISTINCT autor FROM libros
WHERE autor LIKE '%ez';
```

**📝 Patrones en LIKE:**
- `%` representa cero o más caracteres
- `_` representa exactamente un carácter
- `'El%'` encuentra textos que comienzan con "El"
- `'%amor%'` encuentra textos que contienen "amor"
- `'%ez'` encuentra textos que terminan con "ez"
- `DISTINCT` elimina duplicados

### 1.7 Ordenar Resultados (ORDER BY)

**Concepto:** ORDER BY ordena los resultados según una o más columnas.

```sql
-- Ordenar por título alfabéticamente (A-Z)
SELECT titulo, autor FROM libros
ORDER BY titulo ASC;

-- Ordenar por precio de mayor a menor
SELECT titulo, precio FROM libros
ORDER BY precio DESC;

-- Ordenar por múltiples campos
SELECT titulo, autor, año_publicacion FROM libros
ORDER BY año_publicacion DESC, titulo ASC;
```

**📝 Nota:**
- `ASC` = ascendente (de menor a mayor, A-Z)
- `DESC` = descendente (de mayor a menor, Z-A)
- Si no especificas, por defecto es ASC
- Puedes ordenar por múltiples columnas separándolas con comas

### 1.8 Limitar Resultados (LIMIT)

**Concepto:** LIMIT restringe la cantidad de registros devueltos.

```sql
-- Mostrar solo los 3 primeros libros
SELECT titulo, autor FROM libros
LIMIT 3;

-- Los 5 libros más caros
SELECT titulo, precio FROM libros
ORDER BY precio DESC
LIMIT 5;

-- Paginación: saltar los primeros 2 y mostrar los siguientes 3
SELECT titulo, autor FROM libros
LIMIT 3 OFFSET 2;
```

**📝 Nota práctica:**
- Útil para paginación de resultados
- `LIMIT n OFFSET m` salta m registros y muestra n
- Combinar con ORDER BY para resultados consistentes

### 1.9 Funciones de Agregación

**Concepto:** Las funciones de agregación realizan cálculos sobre conjuntos de datos.

```sql
-- Contar cuántos libros tenemos
SELECT COUNT(*) AS total_libros FROM libros;

-- Contar libros leídos
SELECT COUNT(*) AS libros_leidos FROM libros
WHERE leido = TRUE;

-- Precio promedio de los libros
SELECT AVG(precio) AS precio_promedio FROM libros;

-- Precio del libro más caro y más barato
SELECT 
    MAX(precio) AS libro_mas_caro,
    MIN(precio) AS libro_mas_barato
FROM libros;

-- Suma total del valor de la biblioteca
SELECT SUM(precio) AS valor_total FROM libros;

-- Estadísticas completas
SELECT 
    COUNT(*) AS total_libros,
    AVG(precio) AS precio_promedio,
    SUM(precio) AS valor_total,
    MAX(numero_paginas) AS libro_mas_largo
FROM libros;
```

**📝 Funciones principales:**
- `COUNT()` cuenta registros
- `SUM()` suma valores
- `AVG()` calcula promedio
- `MAX()` encuentra el valor máximo
- `MIN()` encuentra el valor mínimo

### 1.10 Agrupar Datos (GROUP BY)

**Concepto:** GROUP BY agrupa registros con valores comunes y permite calcular estadísticas por grupo.

```sql
-- Contar libros por género
SELECT 
    genero,
    COUNT(*) AS cantidad
FROM libros
GROUP BY genero;

-- Precio promedio por género
SELECT 
    genero,
    AVG(precio) AS precio_promedio,
    COUNT(*) AS cantidad_libros
FROM libros
GROUP BY genero;

-- Total de páginas leídas por año de publicación
SELECT 
    año_publicacion,
    SUM(numero_paginas) AS total_paginas
FROM libros
WHERE leido = TRUE
GROUP BY año_publicacion
ORDER BY año_publicacion;
```

**📝 Nota importante:**
- GROUP BY agrupa filas con el mismo valor
- Las columnas en SELECT deben estar en GROUP BY o ser funciones de agregación
- Combina bien con funciones como COUNT, SUM, AVG

### 1.11 Filtrar Grupos (HAVING)

**Concepto:** HAVING filtra grupos después de aplicar GROUP BY (WHERE filtra antes de agrupar).

```sql
-- Géneros con más de 1 libro
SELECT 
    genero,
    COUNT(*) AS cantidad
FROM libros
GROUP BY genero
HAVING COUNT(*) > 1;

-- Géneros con precio promedio mayor a 250
SELECT 
    genero,
    AVG(precio) AS precio_promedio
FROM libros
GROUP BY genero
HAVING AVG(precio) > 250;
```

**📝 Diferencia clave:**
- `WHERE` filtra registros individuales ANTES de agrupar
- `HAVING` filtra grupos DESPUÉS de agrupar
- `HAVING` puede usar funciones de agregación, `WHERE` no

### 1.12 Actualizar Datos (UPDATE)

**Concepto:** UPDATE modifica registros existentes.

```sql
-- Marcar un libro como leído
UPDATE libros
SET leido = TRUE
WHERE titulo = '1984';

-- Actualizar múltiples campos
UPDATE libros
SET 
    precio = 320.00,
    fecha_adquisicion = '2024-04-01'
WHERE titulo = 'Don Quijote de la Mancha';

-- Aumentar el precio de todos los libros en 10%
UPDATE libros
SET precio = precio * 1.10;

-- Actualizar basado en condición
UPDATE libros
SET genero = 'Clásico'
WHERE año_publicacion < 1950;
```

**⚠️ PRECAUCIÓN:**
- Siempre usa WHERE para no actualizar todos los registros
- Sin WHERE, UPDATE modifica TODOS los registros
- Es buena práctica hacer un SELECT con el mismo WHERE antes de actualizar

### 1.13 Eliminar Datos (DELETE)

**Concepto:** DELETE elimina registros de una tabla.

```sql
-- Eliminar un libro específico
DELETE FROM libros
WHERE titulo = 'Libro a eliminar';

-- Eliminar libros con precio menor a 100
DELETE FROM libros
WHERE precio < 100;

-- Eliminar libros antiguos que no hemos leído
DELETE FROM libros
WHERE año_publicacion < 1900 AND leido = FALSE;
```

**⚠️ PRECAUCIÓN CRÍTICA:**
- DELETE sin WHERE elimina TODOS los registros
- No hay deshacer, los datos se pierden permanentemente
- Siempre haz un SELECT primero para verificar qué se eliminará
- Considera hacer un respaldo antes de eliminar

---

## 🎯 Proyecto 2: Sistema de Gestión de Tareas

Vamos a crear un sistema más complejo con múltiples tablas relacionadas.

### 2.1 Diseño de Múltiples Tablas

**Concepto:** Las bases de datos relacionales usan múltiples tablas conectadas entre sí para organizar mejor la información.

```sql
-- Crear base de datos
CREATE DATABASE gestion_tareas;
USE gestion_tareas;

-- Tabla de usuarios
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    fecha_registro DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de proyectos
CREATE TABLE proyectos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(150) NOT NULL,
    descripcion TEXT,
    usuario_id INT,
    fecha_inicio DATE,
    fecha_fin DATE,
    estado ENUM('planificado', 'en_progreso', 'completado', 'cancelado') DEFAULT 'planificado',
    FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);

-- Tabla de tareas
CREATE TABLE tareas (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(200) NOT NULL,
    descripcion TEXT,
    proyecto_id INT,
    usuario_asignado INT,
    prioridad ENUM('baja', 'media', 'alta', 'urgente') DEFAULT 'media',
    estado ENUM('pendiente', 'en_progreso', 'completada', 'cancelada') DEFAULT 'pendiente',
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP,
    fecha_vencimiento DATE,
    horas_estimadas DECIMAL(5, 2),
    horas_reales DECIMAL(5, 2),
    FOREIGN KEY (proyecto_id) REFERENCES proyectos(id),
    FOREIGN KEY (usuario_asignado) REFERENCES usuarios(id)
);
```

**📝 Conceptos nuevos:**
- `TEXT`: Para textos largos sin límite fijo
- `DATETIME`: Fecha y hora
- `CURRENT_TIMESTAMP`: Fecha y hora actual automática
- `UNIQUE`: Garantiza que no haya valores duplicados
- `ENUM`: Lista de valores permitidos
- `FOREIGN KEY`: Crea relación con otra tabla

**📝 Relaciones entre tablas:**
- Un usuario puede tener muchos proyectos (1:N)
- Un proyecto pertenece a un usuario (N:1)
- Un proyecto puede tener muchas tareas (1:N)
- Una tarea pertenece a un proyecto (N:1)

### 2.2 Insertar Datos Relacionados

```sql
-- Insertar usuarios
INSERT INTO usuarios (nombre, email) VALUES
    ('Ana García', 'ana.garcia@email.com'),
    ('Carlos López', 'carlos.lopez@email.com'),
    ('María Rodríguez', 'maria.rodriguez@email.com');

-- Insertar proyectos (relacionados con usuarios)
INSERT INTO proyectos (nombre, descripcion, usuario_id, fecha_inicio, estado) VALUES
    ('Sitio Web Corporativo', 'Desarrollo del nuevo sitio web de la empresa', 1, '2024-01-15', 'en_progreso'),
    ('App Móvil', 'Aplicación móvil para gestión de inventario', 2, '2024-02-01', 'en_progreso'),
    ('Sistema de Reportes', 'Sistema automatizado de reportes', 1, '2024-03-10', 'planificado');

-- Insertar tareas (relacionadas con proyectos y usuarios)
INSERT INTO tareas (titulo, descripcion, proyecto_id, usuario_asignado, prioridad, fecha_vencimiento, horas_estimadas) VALUES
    ('Diseño de interfaz', 'Crear mockups de la página principal', 1, 1, 'alta', '2024-04-15', 16.00),
    ('Configurar base de datos', 'Instalar y configurar MySQL', 1, 2, 'urgente', '2024-04-10', 8.00),
    ('Desarrollo backend', 'API REST con Node.js', 2, 2, 'alta', '2024-04-20', 40.00),
    ('Testing de app', 'Pruebas en dispositivos iOS y Android', 2, 3, 'media', '2024-04-25', 20.00);
```

### 2.3 Consultas con JOIN - Unir Tablas

**Concepto:** JOIN combina filas de dos o más tablas basándose en una relación entre ellas.

#### INNER JOIN

**Concepto:** Devuelve solo los registros que tienen coincidencias en ambas tablas.

```sql
-- Listar tareas con el nombre del proyecto
SELECT 
    tareas.titulo,
    tareas.prioridad,
    proyectos.nombre AS proyecto
FROM tareas
INNER JOIN proyectos ON tareas.proyecto_id = proyectos.id;

-- Listar tareas con proyecto y usuario asignado
SELECT 
    tareas.titulo AS tarea,
    proyectos.nombre AS proyecto,
    usuarios.nombre AS asignado_a,
    tareas.prioridad,
    tareas.fecha_vencimiento
FROM tareas
INNER JOIN proyectos ON tareas.proyecto_id = proyectos.id
INNER JOIN usuarios ON tareas.usuario_asignado = usuarios.id;

-- Tareas del proyecto 'Sitio Web Corporativo'
SELECT 
    t.titulo,
    u.nombre AS asignado,
    t.prioridad,
    t.estado
FROM tareas t
INNER JOIN proyectos p ON t.proyecto_id = p.id
INNER JOIN usuarios u ON t.usuario_asignado = u.id
WHERE p.nombre = 'Sitio Web Corporativo';
```

**📝 Nota sobre alias:**
- Usamos alias de tabla para simplificar: `tareas t`
- Esto hace las consultas más legibles y cortas
- El alias se puede usar inmediatamente después de definirlo

#### LEFT JOIN

**Concepto:** Devuelve todos los registros de la tabla izquierda y los coincidentes de la derecha. Si no hay coincidencia, muestra NULL.

```sql
-- Todos los proyectos con sus tareas (incluso si no tienen tareas)
SELECT 
    p.nombre AS proyecto,
    COUNT(t.id) AS cantidad_tareas
FROM proyectos p
LEFT JOIN tareas t ON p.id = t.proyecto_id
GROUP BY p.id, p.nombre;

-- Usuarios y cantidad de tareas asignadas (incluso si no tienen tareas)
SELECT 
    u.nombre AS usuario,
    COUNT(t.id) AS tareas_asignadas
FROM usuarios u
LEFT JOIN tareas t ON u.id = t.usuario_asignado
GROUP BY u.id, u.nombre;
```

**📝 Diferencia clave:**
- `INNER JOIN`: Solo registros con coincidencias
- `LEFT JOIN`: Todos los registros de la tabla izquierda + coincidencias
- Si no hay coincidencia, los campos de la derecha serán NULL

### 2.4 Subconsultas

**Concepto:** Una consulta dentro de otra consulta.

```sql
-- Usuarios con más tareas que el promedio
SELECT nombre
FROM usuarios
WHERE id IN (
    SELECT usuario_asignado
    FROM tareas
    GROUP BY usuario_asignado
    HAVING COUNT(*) > (SELECT AVG(conteo) 
                       FROM (SELECT COUNT(*) AS conteo 
                             FROM tareas 
                             GROUP BY usuario_asignado) AS subconsulta)
);

-- Proyectos que tienen tareas urgentes
SELECT nombre
FROM proyectos
WHERE id IN (
    SELECT proyecto_id
    FROM tareas
    WHERE prioridad = 'urgente'
);

-- Tareas que superan las horas estimadas promedio
SELECT titulo, horas_estimadas
FROM tareas
WHERE horas_estimadas > (SELECT AVG(horas_estimadas) FROM tareas);
```

**📝 Tipos de subconsultas:**
- Subconsulta escalar: Devuelve un solo valor
- Subconsulta de fila: Devuelve una fila
- Subconsulta de tabla: Devuelve múltiples filas
- Se pueden usar con IN, EXISTS, operadores de comparación

### 2.5 Consultas Avanzadas con Múltiples Joins

```sql
-- Reporte completo: Usuario, Proyecto, Tareas
SELECT 
    u.nombre AS usuario,
    p.nombre AS proyecto,
    COUNT(t.id) AS total_tareas,
    SUM(CASE WHEN t.estado = 'completada' THEN 1 ELSE 0 END) AS completadas,
    SUM(CASE WHEN t.estado = 'pendiente' THEN 1 ELSE 0 END) AS pendientes,
    SUM(t.horas_estimadas) AS horas_estimadas_total,
    SUM(t.horas_reales) AS horas_reales_total
FROM usuarios u
INNER JOIN proyectos p ON u.id = p.usuario_id
LEFT JOIN tareas t ON p.id = t.proyecto_id
GROUP BY u.id, u.nombre, p.id, p.nombre;

-- Tareas urgentes próximas a vencer
SELECT 
    t.titulo,
    p.nombre AS proyecto,
    u.nombre AS responsable,
    t.fecha_vencimiento,
    DATEDIFF(t.fecha_vencimiento, CURDATE()) AS dias_restantes
FROM tareas t
INNER JOIN proyectos p ON t.proyecto_id = p.id
INNER JOIN usuarios u ON t.usuario_asignado = u.id
WHERE t.prioridad = 'urgente' 
  AND t.estado != 'completada'
  AND t.fecha_vencimiento >= CURDATE()
ORDER BY t.fecha_vencimiento ASC;
```

**📝 Funciones útiles:**
- `CASE WHEN`: Condicionales dentro de consultas
- `DATEDIFF()`: Diferencia entre fechas
- `CURDATE()`: Fecha actual
- Combinar todo para reportes complejos

---

## 🎯 Proyecto 3: Tienda Online

Sistema completo de e-commerce para practicar conceptos avanzados.

### 3.1 Estructura Completa de la Base de Datos

```sql
CREATE DATABASE tienda_online;
USE tienda_online;

-- Tabla de clientes
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    telefono VARCHAR(20),
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de categorías
CREATE TABLE categorias (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL UNIQUE,
    descripcion TEXT
);

-- Tabla de productos
CREATE TABLE productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(200) NOT NULL,
    descripcion TEXT,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    categoria_id INT,
    activo BOOLEAN DEFAULT TRUE,
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id),
    CHECK (precio >= 0),
    CHECK (stock >= 0)
);

-- Tabla de pedidos
CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    fecha_pedido TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    estado ENUM('pendiente', 'procesando', 'enviado', 'entregado', 'cancelado') DEFAULT 'pendiente',
    total DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

-- Tabla de detalles del pedido (relación muchos a muchos)
CREATE TABLE detalle_pedido (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT NOT NULL,
    producto_id INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10, 2) NOT NULL,
    subtotal DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (producto_id) REFERENCES productos(id),
    CHECK (cantidad > 0)
);
```

**📝 Conceptos nuevos:**
- `CHECK`: Restricción para validar datos
- `TIMESTAMP`: Marca de tiempo con zona horaria
- Relación muchos a muchos mediante tabla intermedia
- Tabla de detalles para guardar historial de precios

### 3.2 Poblar la Base de Datos

```sql
-- Insertar categorías
INSERT INTO categorias (nombre, descripcion) VALUES
    ('Electrónica', 'Dispositivos electrónicos y accesorios'),
    ('Ropa', 'Prendas de vestir para todas las edades'),
    ('Hogar', 'Artículos para el hogar y decoración'),
    ('Deportes', 'Equipamiento deportivo y fitness');

-- Insertar clientes
INSERT INTO clientes (nombre, apellido, email, telefono) VALUES
    ('Juan', 'Pérez', 'juan.perez@email.com', '555-0001'),
    ('María', 'González', 'maria.gonzalez@email.com', '555-0002'),
    ('Pedro', 'Martínez', 'pedro.martinez@email.com', '555-0003');

-- Insertar productos
INSERT INTO productos (nombre, descripcion, precio, stock, categoria_id) VALUES
    ('Laptop HP', 'Laptop HP 15.6", 8GB RAM, 256GB SSD', 12999.99, 15, 1),
    ('Mouse Inalámbrico', 'Mouse óptico inalámbrico ergonómico', 299.99, 50, 1),
    ('Camiseta Deportiva', 'Camiseta dry-fit para ejercicio', 399.99, 100, 2),
    ('Pantalón Jeans', 'Pantalón de mezclilla clásico', 599.99, 75, 2),
    ('Lámpara LED', 'Lámpara de escritorio LED ajustable', 449.99, 30, 3),
    ('Mancuernas 5kg', 'Par de mancuernas de 5kg cada una', 499.99, 25, 4);

-- Insertar pedidos y detalles
INSERT INTO pedidos (cliente_id, estado, total) VALUES
    (1, 'entregado', 13599.97),
    (2, 'enviado', 1399.97),
    (3, 'pendiente', 999.98);

-- Detalles del pedido 1
INSERT INTO detalle_pedido (pedido_id, producto_id, cantidad, precio_unitario, subtotal) VALUES
    (1, 1, 1, 12999.99, 12999.99),
    (1, 2, 2, 299.99, 599.98);

-- Detalles del pedido 2
INSERT INTO detalle_pedido (pedido_id, producto_id, cantidad, precio_unitario, subtotal) VALUES
    (2, 3, 2, 399.99, 799.98),
    (2, 2, 2, 299.99, 599.98);

-- Detalles del pedido 3
INSERT INTO detalle_pedido (pedido_id, producto_id, cantidad, precio_unitario, subtotal) VALUES
    (3, 6, 2, 499.99, 999.98);
```

### 3.3 Consultas de Análisis de Ventas

```sql
-- Productos más vendidos
SELECT 
    p.nombre,
    SUM(dp.cantidad) AS unidades_vendidas,
    SUM(dp.subtotal) AS ingresos_totales
FROM productos p
INNER JOIN detalle_pedido dp ON p.id = dp.producto_id
GROUP BY p.id, p.nombre
ORDER BY unidades_vendidas DESC;

-- Ventas por categoría
SELECT 
    c.nombre AS categoria,
    COUNT(DISTINCT dp.pedido_id) AS pedidos,
    SUM(dp.cantidad) AS productos_vendidos,
    SUM(dp.subtotal) AS ingresos
FROM categorias c
INNER JOIN productos p ON c.id = p.categoria_id
INNER JOIN detalle_pedido dp ON p.id = dp.producto_id
GROUP BY c.id, c.nombre
ORDER BY ingresos DESC;

-- Clientes con mayor valor de compras
SELECT 
    c.nombre,
    c.apellido,
    COUNT(p.id) AS total_pedidos,
    SUM(p.total) AS valor_total_compras,
    AVG(p.total) AS ticket_promedio
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.id, c.nombre, c.apellido
ORDER BY valor_total_compras DESC;

-- Pedido completo con todos los detalles
SELECT 
    p.id AS pedido_numero,
    CONCAT(c.nombre, ' ', c.apellido) AS cliente,
    p.fecha_pedido,
    p.estado,
    prod.nombre AS producto,
    dp.cantidad,
    dp.precio_unitario,
    dp.subtotal
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id
INNER JOIN detalle_pedido dp ON p.id = dp.pedido_id
INNER JOIN productos prod ON dp.producto_id = prod.id
ORDER BY p.id, dp.id;
```

**📝 Funciones de texto:**
- `CONCAT()`: Une textos
- Combina con funciones de agregación para reportes completos

### 3.4 Vistas (Views)

**Concepto:** Una vista es una consulta guardada que actúa como una tabla virtual.

```sql
-- Crear vista de productos con categoría
CREATE VIEW vista_productos AS
SELECT 
    p.id,
    p.nombre,
    p.descripcion,
    p.precio,
    p.stock,
    c.nombre AS categoria,
    p.activo
FROM productos p
INNER JOIN categorias c ON p.categoria_id = c.id;

-- Usar la vista
SELECT * FROM vista_productos WHERE categoria = 'Electrónica';

-- Vista de pedidos completos
CREATE VIEW vista_pedidos_completos AS
SELECT 
    p.id,
    CONCAT(c.nombre, ' ', c.apellido) AS cliente,
    p.fecha_pedido,
    p.estado,
    p.total,
    COUNT(dp.id) AS cantidad_items
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id
INNER JOIN detalle_pedido dp ON p.id = dp.pedido_id
GROUP BY p.id, c.nombre, c.apellido, p.fecha_pedido, p.estado, p.total;

-- Vista de inventario bajo
CREATE VIEW productos_stock_bajo AS
SELECT 
    nombre,
    stock,
    precio
FROM productos
WHERE stock < 20 AND activo = TRUE;
```

**📝 Ventajas de las vistas:**
- Simplifican consultas complejas
- Mejoran la seguridad (ocultan columnas sensibles)
- Facilitan el mantenimiento del código
- Se actualizan automáticamente con los datos base

---

## 📊 Conceptos Avanzados

### 4.1 Índices para Optimización

**Concepto:** Los índices aceleran las búsquedas en tablas grandes.

```sql
-- Crear índice en columna de búsqueda frecuente
CREATE INDEX idx_producto_nombre ON productos(nombre);

-- Índice compuesto (múltiples columnas)
CREATE INDEX idx_pedido_cliente_fecha ON pedidos(cliente_id, fecha_pedido);

-- Ver los índices de una tabla
SHOW INDEX FROM productos;

-- Eliminar un índice
DROP INDEX idx_producto_nombre ON productos;
```

**📝 Cuándo usar índices:**
- Columnas usadas en WHERE frecuentemente
- Columnas usadas en JOIN
- Columnas usadas en ORDER BY
- ⚠️ Los índices ocupan espacio y ralentizan INSERT/UPDATE

### 4.2 Transacciones

**Concepto:** Una transacción agrupa operaciones que deben ejecutarse todas juntas o ninguna (todo o nada).

```sql
-- Ejemplo: Realizar un pedido completo
START TRANSACTION;

-- Insertar el pedido
INSERT INTO pedidos (cliente_id, total, estado) 
VALUES (1, 1599.98, 'pendiente');

-- Obtener el ID del pedido recién creado
SET @pedido_id = LAST_INSERT_ID();

-- Insertar los detalles
INSERT INTO detalle_pedido (pedido_id, producto_id, cantidad, precio_unitario, subtotal)
VALUES 
    (@pedido_id, 2, 2, 299.99, 599.98),
    (@pedido_id, 3, 2, 499.99, 999.98);

-- Actualizar el stock
UPDATE productos SET stock = stock - 2 WHERE id = 2;
UPDATE productos SET stock = stock - 2 WHERE id = 3;

-- Si todo salió bien, confirmar
COMMIT;

-- Si algo salió mal, deshacer todo
-- ROLLBACK;
```

**📝 Propiedades ACID:**
- **A**tomicity: Todo o nada
- **C**onsistency: Los datos quedan en estado válido
- **I**solation: Las transacciones no interfieren entre sí
- **D**urability: Los cambios confirmados son permanentes

### 4.3 Procedimientos Almacenados

**Concepto:** Código SQL reutilizable guardado en la base de datos.

```sql
-- Procedimiento para registrar un nuevo producto
DELIMITER //
CREATE PROCEDURE registrar_producto(
    IN p_nombre VARCHAR(200),
    IN p_descripcion TEXT,
    IN p_precio DECIMAL(10,2),
    IN p_stock INT,
    IN p_categoria_id INT
)
BEGIN
    INSERT INTO productos (nombre, descripcion, precio, stock, categoria_id)
    VALUES (p_nombre, p_descripcion, p_precio, p_stock, p_categoria_id);
    
    SELECT LAST_INSERT_ID() AS nuevo_producto_id;
END //
DELIMITER ;

-- Usar el procedimiento
CALL registrar_producto('Teclado Mecánico', 'Teclado RGB gaming', 899.99, 20, 1);

-- Procedimiento con lógica más compleja
DELIMITER //
CREATE PROCEDURE actualizar_estado_pedido(
    IN p_pedido_id INT,
    IN p_nuevo_estado VARCHAR(20)
)
BEGIN
    DECLARE pedido_existe INT;
    
    -- Verificar si el pedido existe
    SELECT COUNT(*) INTO pedido_existe 
    FROM pedidos 
    WHERE id = p_pedido_id;
    
    IF pedido_existe > 0 THEN
        UPDATE pedidos 
        SET estado = p_nuevo_estado 
        WHERE id = p_pedido_id;
        
        SELECT CONCAT('Pedido ', p_pedido_id, ' actualizado a: ', p_nuevo_estado) AS resultado;
    ELSE
        SELECT 'Pedido no encontrado' AS resultado;
    END IF;
END //
DELIMITER ;

-- Usar el procedimiento
CALL actualizar_estado_pedido(1, 'enviado');
```

**📝 Ventajas de procedimientos almacenados:**
- Código reutilizable
- Mejor rendimiento (código precompilado)
- Lógica centralizada en la base de datos
- Reducen tráfico de red

### 4.4 Triggers (Disparadores)

**Concepto:** Código que se ejecuta automáticamente cuando ocurre un evento (INSERT, UPDATE, DELETE).

```sql
-- Trigger para actualizar stock automáticamente al hacer un pedido
DELIMITER //
CREATE TRIGGER actualizar_stock_pedido
AFTER INSERT ON detalle_pedido
FOR EACH ROW
BEGIN
    UPDATE productos 
    SET stock = stock - NEW.cantidad
    WHERE id = NEW.producto_id;
END //
DELIMITER ;

-- Trigger para validar que hay stock antes de insertar detalle
DELIMITER //
CREATE TRIGGER validar_stock_antes_pedido
BEFORE INSERT ON detalle_pedido
FOR EACH ROW
BEGIN
    DECLARE stock_actual INT;
    
    SELECT stock INTO stock_actual
    FROM productos
    WHERE id = NEW.producto_id;
    
    IF stock_actual < NEW.cantidad THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Stock insuficiente para el producto';
    END IF;
END //
DELIMITER ;

-- Trigger para registrar cambios de precio
CREATE TABLE historial_precios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    producto_id INT,
    precio_anterior DECIMAL(10,2),
    precio_nuevo DECIMAL(10,2),
    fecha_cambio TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER //
CREATE TRIGGER registrar_cambio_precio
AFTER UPDATE ON productos
FOR EACH ROW
BEGIN
    IF OLD.precio != NEW.precio THEN
        INSERT INTO historial_precios (producto_id, precio_anterior, precio_nuevo)
        VALUES (NEW.id, OLD.precio, NEW.precio);
    END IF;
END //
DELIMITER ;
```

**📝 Tipos de triggers:**
- `BEFORE INSERT/UPDATE/DELETE`: Antes de la operación
- `AFTER INSERT/UPDATE/DELETE`: Después de la operación
- `OLD`: Valores anteriores (UPDATE/DELETE)
- `NEW`: Valores nuevos (INSERT/UPDATE)

---

## 🎓 Ejercicios Prácticos

### Ejercicio 1: Biblioteca (Básico)
1. Agrega 5 libros más a tu biblioteca
2. Encuentra todos los libros publicados entre 1950 y 2000
3. Calcula el valor total de tu biblioteca
4. Lista los géneros y cuántos libros tienes de cada uno
5. Actualiza el precio de todos los libros aumentándolo un 15%

### Ejercicio 2: Gestión de Tareas (Intermedio)
1. Crea 2 usuarios nuevos
2. Crea un proyecto para cada usuario
3. Asigna 3 tareas a cada proyecto
4. Encuentra todas las tareas que vencen en los próximos 7 días
5. Calcula el tiempo total estimado vs real por proyecto
6. Lista los usuarios con más tareas pendientes

### Ejercicio 3: Tienda Online (Avanzado)
1. Crea 5 productos nuevos en diferentes categorías
2. Registra 3 pedidos nuevos con múltiples productos
3. Crea una vista que muestre productos con bajo stock
4. Encuentra el producto más rentable (mayor ingreso total)
5. Lista los clientes que no han realizado pedidos
6. Calcula el valor promedio de pedido por cliente

---

## 💡 Mejores Prácticas

### Nomenclatura
- Usa nombres descriptivos en inglés o español (consistente)
- Nombres de tablas en plural: `productos`, `usuarios`
- Nombres de columnas en singular: `nombre`, `precio`
- Snake_case para nombres: `fecha_creacion`, `usuario_id`

### Seguridad
```sql
-- ✅ BIEN: Usar prepared statements (en código de aplicación)
-- ❌ MAL: Concatenar valores directamente (riesgo de SQL injection)

-- Siempre validar datos antes de INSERT/UPDATE
-- Usar FOREIGN KEY para mantener integridad referencial
-- Limitar permisos de usuario según necesidad
```

### Performance
```sql
-- Usar EXPLAIN para ver el plan de ejecución
EXPLAIN SELECT * FROM productos WHERE categoria_id = 1;

-- Crear índices en columnas de búsqueda frecuente
-- Evitar SELECT * cuando no necesitas todas las columnas
-- Usar LIMIT para grandes conjuntos de datos
-- Optimizar JOIN usando índices apropiados
```

### Mantenimiento
```sql
-- Hacer respaldos regulares
mysqldump -u usuario -p nombre_bd > respaldo.sql

-- Restaurar desde respaldo
mysql -u usuario -p nombre_bd < respaldo.sql

-- Ver tamaño de tablas
SELECT 
    table_name,
    ROUND((data_length + index_length) / 1024 / 1024, 2) AS size_mb
FROM information_schema.tables
WHERE table_schema = 'nombre_base_datos';
```

---

## 🚀 Próximos Pasos

Ahora que dominas SQL, puedes:

1. **Conectar con lenguajes de programación:**
   - Python (MySQL Connector, SQLAlchemy)
   - JavaScript (mysql2, Sequelize)
   - PHP (PDO, MySQLi)
   - Java (JDBC)

2. **Explorar bases de datos avanzadas:**
   - PostgreSQL (más funciones avanzadas)
   - MongoDB (NoSQL)
   - Redis (caché)

3. **Herramientas útiles:**
   - phpMyAdmin (interfaz web)
   - MySQL Workbench (cliente gráfico)
   - DBeaver (cliente universal)

4. **Conceptos avanzados:**
   - Particionamiento de tablas
   - Replicación
   - Optimización de consultas complejas
   - Data warehousing

---

## 📚 Recursos Adicionales

- Documentación oficial de MySQL: https://dev.mysql.com/doc/
- Práctica interactiva: SQLZoo, LeetCode SQL
- Cursos complementarios: Database Design, Data Modeling

---

## 🎯 Checklist de Aprendizaje

- [ ] Crear bases de datos y tablas
- [ ] Insertar, actualizar y eliminar datos
- [ ] Consultas básicas con WHERE, ORDER BY, LIMIT
- [ ] Funciones de agregación (COUNT, SUM, AVG, MAX, MIN)
- [ ] Agrupar datos con GROUP BY y HAVING
- [ ] Unir tablas con INNER JOIN y LEFT JOIN
- [ ] Usar subconsultas
- [ ] Crear vistas
- [ ] Implementar transacciones
- [ ] Crear procedimientos almacenados
- [ ] Usar triggers
- [ ] Optimizar con índices
- [ ] Completar los 3 proyectos prácticos

---

**¡Felicidades por completar el curso práctico de SQL!** 🎉

Recuerda: La mejor manera de aprender es practicando. Crea tus propios proyectos, experimenta con diferentes consultas y no tengas miedo de cometer errores. ¡La práctica hace al maestro!
