# Fundamentos de React.js - Guía de 1 Hora

Aprenderás los conceptos fundamentales de React.js en solo 1 hora! 🚀

## Requisitos Previos
- Conocimientos básicos de HTML, CSS y JavaScript
- Node.js y npm instalados
- Un editor de código (VS Code recomendado)

## 1. Configuración del Proyecto (5 minutos)

```bash
# Crear un nuevo proyecto con Create React App
npx create-react-app mi-primera-app

# Entrar al directorio
cd mi-primera-app

# Iniciar el servidor de desarrollo
npm start
```

## 2. Componentes Funcionales (10 minutos)

```jsx
// src/components/Saludo.js
import React from 'react';

function Saludo({ nombre = "Usuario" }) {
  return (
    <div className="saludo">
      <h1>¡Hola, {nombre}!</h1>
      <p>Bienvenido a React</p>
    </div>
  );
}

export default Saludo;

// App.js - Uso del componente
import Saludo from './components/Saludo';

function App() {
  return (
    <div className="App">
      <Saludo nombre="Juan" />
      <Saludo nombre="María" />
      <Saludo /> {/* Usará el valor por defecto */}
    </div>
  );
}
```

## 3. Estado con useState (10 minutos)

```jsx
// src/components/Contador.js
import React, { useState } from 'react';

function Contador() {
  const [contador, setContador] = useState(0);
  const [mensaje, setMensaje] = useState('');

  const incrementar = () => {
    setContador(contador + 1);
  };

  const decrementar = () => {
    setContador(prevContador => prevContador - 1);
  };

  return (
    <div>
      <h2>Contador: {contador}</h2>
      <button onClick={incrementar}>Incrementar</button>
      <button onClick={decrementar}>Decrementar</button>
      
      <br />
      <input 
        type="text"
        value={mensaje}
        onChange={(e) => setMensaje(e.target.value)}
        placeholder="Escribe algo..."
      />
      <p>Mensaje: {mensaje}</p>
    </div>
  );
}

export default Contador;
```

## 4. Efectos con useEffect (10 minutos)

```jsx
// src/components/Timer.js
import React, { useState, useEffect } from 'react';

function Timer() {
  const [segundos, setSegundos] = useState(0);
  const [activo, setActivo] = useState(false);

  useEffect(() => {
    let intervalo = null;
    
    if (activo) {
      intervalo = setInterval(() => {
        setSegundos(segundos => segundos + 1);
      }, 1000);
    }

    // Cleanup function
    return () => clearInterval(intervalo);
  }, [activo]);

  return (
    <div>
      <h2>Temporizador: {segundos} segundos</h2>
      <button onClick={() => setActivo(!activo)}>
        {activo ? 'Pausar' : 'Iniciar'}
      </button>
      <button onClick={() => setSegundos(0)}>Reiniciar</button>
    </div>
  );
}

export default Timer;
```

## 5. Manejo de Formularios (10 minutos)

```jsx
// src/components/Formulario.js
import React, { useState } from 'react';

function Formulario() {
  const [formData, setFormData] = useState({
    nombre: '',
    email: '',
    mensaje: ''
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prevState => ({
      ...prevState,
      [name]: value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Datos del formulario:', formData);
    // Aquí irían las validaciones y el envío del formulario
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Nombre:</label>
        <input
          type="text"
          name="nombre"
          value={formData.nombre}
          onChange={handleChange}
        />
      </div>
      
      <div>
        <label>Email:</label>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
      </div>
      
      <div>
        <label>Mensaje:</label>
        <textarea
          name="mensaje"
          value={formData.mensaje}
          onChange={handleChange}
        />
      </div>
      
      <button type="submit">Enviar</button>
    </form>
  );
}

export default Formulario;
```

## 6. Lista de Tareas - Proyecto Práctico (15 minutos)

```jsx
// src/components/TodoList.js
import React, { useState } from 'react';
import './TodoList.css';

function TodoList() {
  const [tareas, setTareas] = useState([]);
  const [nuevaTarea, setNuevaTarea] = useState('');

  const agregarTarea = (e) => {
    e.preventDefault();
    if (nuevaTarea.trim()) {
      setTareas([...tareas, {
        id: Date.now(),
        texto: nuevaTarea,
        completada: false
      }]);
      setNuevaTarea('');
    }
  };

  const toggleTarea = (id) => {
    setTareas(tareas.map(tarea => 
      tarea.id === id 
        ? { ...tarea, completada: !tarea.completada }
        : tarea
    ));
  };

  const eliminarTarea = (id) => {
    setTareas(tareas.filter(tarea => tarea.id !== id));
  };

  return (
    <div className="todo-container">
      <h1>Lista de Tareas</h1>
      
      <form onSubmit={agregarTarea}>
        <input
          type="text"
          value={nuevaTarea}
          onChange={(e) => setNuevaTarea(e.target.value)}
          placeholder="Nueva tarea..."
        />
        <button type="submit">Agregar</button>
      </form>

      <ul className="todo-list">
        {tareas.map(tarea => (
          <li key={tarea.id} className={tarea.completada ? 'completada' : ''}>
            <input
              type="checkbox"
              checked={tarea.completada}
              onChange={() => toggleTarea(tarea.id)}
            />
            <span>{tarea.texto}</span>
            <button onClick={() => eliminarTarea(tarea.id)}>
              Eliminar
            </button>
          </li>
        ))}
      </ul>

      <div className="todo-stats">
        <p>Total: {tareas.length}</p>
        <p>Completadas: {tareas.filter(t => t.completada).length}</p>
        <p>Pendientes: {tareas.filter(t => !t.completada).length}</p>
      </div>
    </div>
  );
}

// src/components/TodoList.css
.todo-container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.completada span {
  text-decoration: line-through;
  color: #888;
}

.todo-stats {
  margin-top: 20px;
  padding: 10px;
  background: #f5f5f5;
  border-radius: 5px;
}
```

## Buenas Prácticas y Tips

1. Mantén los componentes pequeños y enfocados
2. Usa nombres descriptivos para componentes y funciones
3. Evita la mutación directa del estado
4. Extrae la lógica compleja a hooks personalizados
5. Utiliza PropTypes o TypeScript para type checking
6. Sigue el principio de componentes controlados para formularios

## Recursos Adicionales
- [Documentación oficial de React](https://reactjs.org/docs/getting-started.html)
- [Create React App](https://create-react-app.dev/)
- [React DevTools](https://chrome.google.com/webstore/detail/react-developer-tools/)
- [Awesome React](https://github.com/enaqx/awesome-react)

¡Felicitaciones! 🎉 Has completado el tutorial de fundamentos de React. Ahora tienes las bases necesarias para comenzar a desarrollar aplicaciones con React.
