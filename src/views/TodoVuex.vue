<template>
  <h1>Lista de tareas de Thanos</h1>
  <h4>Tareas: {{ pending.length }}</h4>

  <hr>
  <button @click="currentTab = 'all'" :class="{'active': currentTab === 'all'}">Todos</button>
  <button @click="currentTab = 'pending'" :class="{'active': currentTab === 'pending'}">Pendientes</button>
  <button @click="currentTab = 'completed'" :class="{'active': currentTab === 'completed'}">Completados</button>

  <div>
    <ul>
      <li v-for="todo in getTodosByTab" :key="todo.id" :class="{'completed': todo.completed}" @dblclick="toggleTodo(todo.id)">
        {{ todo.text }}
      </li>
    </ul>
  </div>

  <button @click="isOpen = true">Crear Todo</button>
  <modal v-if="isOpen" @on:close="isOpen = false">

    <template v-slot:header>
      <h2>Nueva Tarea</h2>
    </template>

    <template v-slot:body>
      <form @submit.prevent="createTodo(newTodoText); isOpen = false">
        <input type="text" placeholder="Nueva tarea" v-model="newTodoText">
        <br>
        <br>
        <button type="submit">Crear Tarea</button>
      </form>
    </template>

  </modal>

</template>

<script>
import { ref } from 'vue'
import useTodos from '@/composables/useTodos'
import Modal from '@/components/Modal.vue'

export default {
  components: { Modal },
  setup(){
    const { currentTab, pending, getTodosByTab, toggleTodo, createTodo} = useTodos()
    
    return {
      createTodo,
      currentTab,
      pending,
      getTodosByTab,
      toggleTodo,

      isOpen: ref(false),
      newTodoText: ref('')
    }
  }
}
</script>

<style scoped>
div {
  display: flex;
  justify-content: center;
  text-align: center;
}

ul{
  width: 300px;
  text-align: left;
}

li{
  cursor: pointer;
}

.active{
  background-color: #2c3e50;
  color: white;
}

.completed{
  text-decoration: line-through;
}
</style>