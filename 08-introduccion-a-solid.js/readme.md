# Introduccion a Solid.js - Guía de 1 Hora

Aprenderás los conceptos fundamentales de Solid.js, un framework moderno y reactivo para construir aplicaciones web. 🚀

## Requisitos Previos
- Node.js instalado
- Conocimientos básicos de JavaScript/TypeScript
- Familiaridad con React (útil pero no necesario)

## 1. Configuración Inicial (5 minutos)

```bash
# Crear un nuevo proyecto Solid
npx degit solidjs/templates/js mi-app-solid
cd mi-app-solid

# Instalar dependencias
npm install

# Iniciar servidor de desarrollo
npm run dev
```

## 2. Componentes y Señales (15 minutos)

### Componentes Básicos
```jsx
// App.jsx
import { createSignal } from 'solid-js';

function Counter() {
  // Crear una señal (estado reactivo)
  const [count, setCount] = createSignal(0);
  
  return (
    <div>
      <p>Contador: {count()}</p>
      <button onClick={() => setCount(count() + 1)}>
        Incrementar
      </button>
    </div>
  );
}

// Con props
function Greeting(props) {
  return <h1>Hola, {props.name}!</h1>;
}
```

> **Nota sobre Componentes:**
> - Los componentes son funciones que retornan JSX
> - Las señales son el sistema de reactividad principal
> - `createSignal` retorna un getter y un setter
> - Los getters deben ser llamados como funciones: `count()`
> - Props son inmutables

### Señales y Reactividad
```jsx
import { createSignal, createEffect } from 'solid-js';

function ReactiveExample() {
  const [name, setName] = createSignal("Juan");
  const [age, setAge] = createSignal(25);
  
  // Efecto que se ejecuta cuando las dependencias cambian
  createEffect(() => {
    console.log(`${name()} tiene ${age()} años`);
  });
  
  return (
    <div>
      <input
        value={name()}
        onInput={(e) => setName(e.target.value)}
      />
      <button onClick={() => setAge(a => a + 1)}>
        Cumplir años
      </button>
    </div>
  );
}
```

> **Nota sobre Señales:**
> - Las señales son el corazón de la reactividad en Solid
> - `createEffect` crea un efecto secundario reactivo
> - Los efectos se ejecutan automáticamente cuando sus dependencias cambian
> - Las señales pueden ser actualizadas con un valor o una función

## 3. Control de Flujo (10 minutos)

```jsx
import { createSignal, Show, For } from 'solid-js';

function FlowControl() {
  const [isLoggedIn, setIsLoggedIn] = createSignal(false);
  const [items, setItems] = createSignal([
    { id: 1, text: 'Tarea 1' },
    { id: 2, text: 'Tarea 2' }
  ]);

  return (
    <div>
      {/* Renderizado condicional */}
      <Show
        when={isLoggedIn()}
        fallback={<button onClick={() => setIsLoggedIn(true)}>
          Login
        </button>}
      >
        <h2>Bienvenido!</h2>
        <button onClick={() => setIsLoggedIn(false)}>
          Logout
        </button>
      </Show>

      {/* Listas */}
      <ul>
        <For each={items()}>
          {(item) => (
            <li>{item.text}</li>
          )}
        </For>
      </ul>
    </div>
  );
}
```

> **Nota sobre Control de Flujo:**
> - `Show` es similar a un if-else
> - `For` es optimizado para renderizar listas
> - No usar map directamente como en React
> - Los componentes de control de flujo son más eficientes

## 4. Recursos y Stores (10 minutos)

```jsx
import { createResource, createStore } from 'solid-js';

// Recurso para datos asíncronos
function UserProfile() {
  const [user, { refetch }] = createResource(async () => {
    const response = await fetch('https://api.example.com/user');
    return response.json();
  });

  return (
    <div>
      <Show when={!user.loading} fallback={<div>Cargando...</div>}>
        <h2>{user()?.name}</h2>
        <p>{user()?.email}</p>
      </Show>
      <button onClick={refetch}>Actualizar</button>
    </div>
  );
}

// Store para objetos complejos
function TodoList() {
  const [todos, setTodos] = createStore({
    items: [],
    filter: 'all'
  });

  const addTodo = (text) => {
    setTodos('items', (items) => [...items, {
      id: Date.now(),
      text,
      completed: false
    }]);
  };

  return (
    <div>
      <input
        onKeyPress={(e) => {
          if (e.key === 'Enter') {
            addTodo(e.target.value);
            e.target.value = '';
          }
        }}
      />
      <For each={todos.items}>
        {(todo) => <div>{todo.text}</div>}
      </For>
    </div>
  );
}
```

> **Nota sobre Recursos y Stores:**
> - `createResource` maneja datos asíncronos
> - Los recursos tienen estados loading/error incorporados
> - `createStore` es para objetos anidados complejos
> - Los stores son más eficientes que las señales para estructuras anidadas

## 5. Proyecto Práctico: Lista de Tareas (20 minutos)

```jsx
// TodoApp.jsx
import { createSignal, createStore, Show, For } from 'solid-js';
import { createEffect } from 'solid-js';

function TodoApp() {
  // Estado local
  const [newTodo, setNewTodo] = createSignal('');
  
  // Store para las tareas
  const [todos, setTodos] = createStore({
    items: [],
    filter: 'all'
  });

  // Tareas filtradas como una señal derivada
  const filteredTodos = () => {
    return todos.items.filter(todo => {
      if (todos.filter === 'completed') return todo.completed;
      if (todos.filter === 'active') return !todo.completed;
      return true;
    });
  };

  // Agregar tarea
  const addTodo = (text) => {
    if (!text.trim()) return;
    setTodos('items', items => [...items, {
      id: Date.now(),
      text,
      completed: false
    }]);
    setNewTodo('');
  };

  // Alternar estado de tarea
  const toggleTodo = (id) => {
    setTodos('items', todo => todo.id === id, 
      'completed', completed => !completed);
  };

  // Eliminar tarea
  const removeTodo = (id) => {
    setTodos('items', items => 
      items.filter(item => item.id !== id));
  };

  // Efecto para guardar en localStorage
  createEffect(() => {
    localStorage.setItem('todos', 
      JSON.stringify(todos.items));
  });

  return (
    <div class="todo-app">
      <h1>Lista de Tareas</h1>
      
      {/* Formulario para agregar tareas */}
      <form onSubmit={(e) => {
        e.preventDefault();
        addTodo(newTodo());
      }}>
        <input
          value={newTodo()}
          onInput={(e) => setNewTodo(e.target.value)}
          placeholder="Nueva tarea..."
        />
        <button type="submit">Agregar</button>
      </form>

      {/* Filtros */}
      <div class="filters">
        <button onClick={() => setTodos('filter', 'all')}>
          Todas
        </button>
        <button onClick={() => setTodos('filter', 'active')}>
          Activas
        </button>
        <button onClick={() => setTodos('filter', 'completed')}>
          Completadas
        </button>
      </div>

      {/* Lista de tareas */}
      <Show
        when={filteredTodos().length > 0}
        fallback={<p>No hay tareas</p>}
      >
        <ul>
          <For each={filteredTodos()}>
            {(todo) => (
              <li class={todo.completed ? 'completed' : ''}>
                <input
                  type="checkbox"
                  checked={todo.completed}
                  onChange={() => toggleTodo(todo.id)}
                />
                <span>{todo.text}</span>
                <button onClick={() => removeTodo(todo.id)}>
                  Eliminar
                </button>
              </li>
            )}
          </For>
        </ul>
      </Show>
    </div>
  );
}

export default TodoApp;
```

```css
/* styles.css */
.todo-app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.filters {
  margin: 20px 0;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-bottom: 1px solid #eee;
}
```

## Recursos Adicionales
- [Documentación oficial de Solid.js](https://www.solidjs.com/)
- [Tutorial Interactivo](https://www.solidjs.com/tutorial/)
- [Solid.js Playground](https://playground.solidjs.com/)
- [Ejemplos de la Comunidad](https://github.com/solidjs/solid/blob/main/documentation/resources/community.md)

¡Felicitaciones! 🎉 Has completado el tutorial de Solid.js. Ahora tienes las bases necesarias para crear aplicaciones reactivas eficientes.
