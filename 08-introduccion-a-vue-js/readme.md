# Fundamentos de Vue.js - Guía de 1 Hora

Aprenderás los conceptos fundamentales de Vue.js en solo 1 hora! 🚀

## Requisitos Previos
- Conocimientos básicos de HTML, CSS y JavaScript
- Node.js y npm instalados
- Un editor de código (VS Code recomendado)

## 1. Configuración del Proyecto (5 minutos)

```bash
# Instalar Vue CLI globalmente
npm install -g @vue/cli

# Crear un nuevo proyecto
vue create mi-primer-proyecto

# Seleccionar Vue 3 y opciones por defecto

# Entrar al directorio y ejecutar
cd mi-primer-proyecto
npm run serve
```

## 2. Componentes Básicos (10 minutos)

### Estructura de un Componente
```vue
<!-- App.vue -->
<template>
  <div class="app">
    <h1>{{ titulo }}</h1>
    <p>{{ descripcion }}</p>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      titulo: 'Mi Primera App Vue',
      descripcion: 'Bienvenido a Vue.js'
    }
  }
}
</script>

<style>
.app {
  text-align: center;
  margin-top: 20px;
}
</style>
```

### Componente Hijo
```vue
<!-- components/Saludo.vue -->
<template>
  <div class="saludo">
    <h2>{{ mensaje }}</h2>
    <button @click="cambiarMensaje">Cambiar Mensaje</button>
  </div>
</template>

<script>
export default {
  name: 'Saludo',
  data() {
    return {
      mensaje: '¡Hola Vue!'
    }
  },
  methods: {
    cambiarMensaje() {
      this.mensaje = '¡Mensaje Cambiado!'
    }
  }
}
</script>
```

## 3. Directivas Básicas (10 minutos)

```vue
<template>
  <div>
    <!-- v-if para renderizado condicional -->
    <div v-if="mostrar">Este contenido es condicional</div>
    <div v-else>Contenido alternativo</div>

    <!-- v-show para alternar visibilidad -->
    <div v-show="visible">Este contenido se oculta/muestra</div>

    <!-- v-for para iteración -->
    <ul>
      <li v-for="(item, index) in items" :key="index">
        {{ item.nombre }}
      </li>
    </ul>

    <!-- v-bind para atributos -->
    <img :src="imagenUrl" :alt="imageAlt">

    <!-- v-on para eventos -->
    <button @click="handleClick">Clickeame</button>

    <!-- v-model para two-way binding -->
    <input v-model="mensaje" type="text">
    <p>El mensaje es: {{ mensaje }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mostrar: true,
      visible: true,
      items: [
        { nombre: 'Item 1' },
        { nombre: 'Item 2' },
        { nombre: 'Item 3' }
      ],
      imagenUrl: 'ruta/imagen.jpg',
      imageAlt: 'Mi imagen',
      mensaje: ''
    }
  },
  methods: {
    handleClick() {
      alert('¡Botón clickeado!')
    }
  }
}
</script>
```

## 4. Props y Eventos (10 minutos)

### Componente Padre
```vue
<template>
  <div>
    <hijo 
      :mensaje="mensajePadre"
      @respuesta="manejarRespuesta"
    />
  </div>
</template>

<script>
import Hijo from './components/Hijo.vue'

export default {
  components: {
    Hijo
  },
  data() {
    return {
      mensajePadre: 'Mensaje desde el padre'
    }
  },
  methods: {
    manejarRespuesta(respuesta) {
      console.log('Respuesta del hijo:', respuesta)
    }
  }
}
</script>
```

### Componente Hijo
```vue
<template>
  <div>
    <p>{{ mensaje }}</p>
    <button @click="responder">Responder al padre</button>
  </div>
</template>

<script>
export default {
  props: {
    mensaje: {
      type: String,
      required: true
    }
  },
  methods: {
    responder() {
      this.$emit('respuesta', '¡Hola padre!')
    }
  }
}
</script>
```

## 5. Computed Properties y Watch (10 minutos)

```vue
<template>
  <div>
    <input v-model="nombre" type="text">
    <input v-model="apellido" type="text">
    
    <!-- Usando computed property -->
    <p>Nombre completo: {{ nombreCompleto }}</p>
    
    <!-- Lista filtrada -->
    <input v-model="busqueda" type="text" placeholder="Buscar...">
    <ul>
      <li v-for="item in itemsFiltrados" :key="item.id">
        {{ item.nombre }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nombre: '',
      apellido: '',
      busqueda: '',
      items: [
        { id: 1, nombre: 'Manzana' },
        { id: 2, nombre: 'Banana' },
        { id: 3, nombre: 'Naranja' }
      ]
    }
  },
  computed: {
    nombreCompleto() {
      return `${this.nombre} ${this.apellido}`
    },
    itemsFiltrados() {
      return this.items.filter(item => 
        item.nombre.toLowerCase().includes(this.busqueda.toLowerCase())
      )
    }
  },
  watch: {
    busqueda(nuevoValor) {
      console.log('Búsqueda cambiada:', nuevoValor)
    }
  }
}
</script>
```

## 6. Ejemplo Práctico: Lista de Tareas (15 minutos)

```vue
<template>
  <div class="todo-app">
    <h1>Lista de Tareas</h1>
    
    <!-- Formulario para agregar tareas -->
    <form @submit.prevent="agregarTarea">
      <input 
        v-model="nuevaTarea" 
        type="text" 
        placeholder="Nueva tarea..."
        required
      >
      <button type="submit">Agregar</button>
    </form>

    <!-- Lista de tareas -->
    <ul class="todo-list">
      <li v-for="tarea in tareas" :key="tarea.id">
        <input 
          type="checkbox" 
          v-model="tarea.completada"
        >
        <span :class="{ completada: tarea.completada }">
          {{ tarea.texto }}
        </span>
        <button @click="eliminarTarea(tarea.id)">
          Eliminar
        </button>
      </li>
    </ul>

    <!-- Estadísticas -->
    <div class="estadisticas">
      <p>Total: {{ totalTareas }}</p>
      <p>Completadas: {{ tareasCompletadas }}</p>
      <p>Pendientes: {{ tareasPendientes }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nuevaTarea: '',
      tareas: []
    }
  },
  computed: {
    totalTareas() {
      return this.tareas.length
    },
    tareasCompletadas() {
      return this.tareas.filter(t => t.completada).length
    },
    tareasPendientes() {
      return this.totalTareas - this.tareasCompletadas
    }
  },
  methods: {
    agregarTarea() {
      if (this.nuevaTarea.trim()) {
        this.tareas.push({
          id: Date.now(),
          texto: this.nuevaTarea,
          completada: false
        })
        this.nuevaTarea = ''
      }
    },
    eliminarTarea(id) {
      this.tareas = this.tareas.filter(t => t.id !== id)
    }
  }
}
</script>

<style>
.todo-app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.completada {
  text-decoration: line-through;
  color: gray;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 10px 0;
}
</style>
```

## Recursos Adicionales
- [Documentación oficial de Vue.js](https://vuejs.org/)
- [Vue CLI Documentation](https://cli.vuejs.org/)
- [Awesome Vue](https://github.com/vuejs/awesome-vue)
- [Vue DevTools](https://devtools.vuejs.org/)

¡Felicitaciones! 🎉 Has completado el tutorial de fundamentos de Vue.js. Ahora tienes las bases necesarias para comenzar a desarrollar aplicaciones con Vue.
