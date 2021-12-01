<script setup lang="ts">
import { ref, computed, version } from 'vue'
import { v4 as uuid } from 'uuid';

import { Todo } from './interfaces/todo';
import TodoCard from './components/TodoCard.vue';

enum Filter {
  All = 'all',
  Open = 'open',
  Closed = 'closed'
}

const todoDraft = ref<Todo>({
  id: uuid(),
  title: '',
  description: '',
  createdAt: new Date(),
  closed: false
})

const todos = ref<Todo[]>([{ 
  id: uuid(), 
  title: 'Test',
  description: 'Test description',
  closed: false,
  createdAt: new Date()
}]);

const filter = ref<Filter>(Filter.All)

function saveTodo(event: Event) {
  event.preventDefault();

  todos.value.push(todoDraft.value);

  todoDraft.value = {
    id: uuid(),
    title: '',
    description: '',
    createdAt: new Date(),
    closed: false
  }
}

function changeStatus(id: string) {
  const todo = todos.value.find(todo => todo.id === id);
  if(!todo) {
    throw new Error(`Unable to fin todo with id: ${id}`);
  } 
  todo.closed = !todo.closed;
}

function deleteTodo(id: string) {
  const index = todos.value.findIndex(todo => todo.id === id);
  todos.value.splice(index, 1);
}

const filteredTodos = computed(() => {
  if(filter.value === Filter.All) {
    return todos.value;
  }

  const filterClosed = filter.value === Filter.Closed;

  return todos.value.filter(todo => todo.closed === filterClosed);
})
</script>

<template>
  <header>
    <hgroup>
      <h1>Basic Todo App</h1>
      <small>A basic Todo App build with Vue {{ version }}</small>
    </hgroup>
  </header>
  <main>
    <form @submit="saveTodo">
      <label for="todo-input">Enter a new Todo</label>
      <input
        id="todo-input"
        v-model="todoDraft.title"
        type="text"
        name="todo-input"
        placeholder="Chores"
        required
      >
      <textarea
        id="todo-textarea"
        v-model="todoDraft.description"
        name="todo-textarea"
        placeholder="Take out the trash"
        required
      />
      <button type="submit">
        Save Todo
      </button>
    </form>
    <hr>
    <section>
      <div class="todo-status-container">
        <div>
          <label for="todo-filter">Filter:</label>
          <select
            id="todo-filter"
            v-model="filter"
          >
            <option value="all">
              All
            </option>
            <option value="open">
              Open
            </option>
            <option value="closed">
              Closed
            </option>
          </select>
        </div>
      </div>
      <todo-card
        v-for="todo in filteredTodos"
        :key="todo.id"
        :todo="todo"
        @change-status="changeStatus"
        @delete="deleteTodo"
      />
    </section>
  </main>
</template>

<style lang="scss" scoped>
@media (min-width: 900px) {
  .todo-status-container {
    display: flex;
    justify-content: flex-end;

    div {
      width: 33%;
    }
  }
}


main {
  padding: 0 var(--block-spacing-horizontal);
}
</style>