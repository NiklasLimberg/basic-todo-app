<script setup lang="ts">
import { ref, computed, version } from 'vue'
import { v4 as uuid } from 'uuid';

enum Filter {
  All = 'all',
  Open = 'open',
  Closed = 'closed'
}

interface Todo {
  id: string,
  createdAt: Date,
  title: string,
  description: string,
  closed: boolean
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

function changeStatus(todo: Todo) {
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
      <article
        v-for="todo in filteredTodos"
        :key="todo.id"
      >
        <header class="position-relative">
          <h1 class="remove-margin">
            {{ todo.title }}
          </h1>
          <div class="float-right">
            <button @click="changeStatus(todo)">
              {{ todo.closed ? 'Open' : 'Close' }}
            </button>
            <button @click="deleteTodo(todo.id)">
              Delete
            </button>
          </div>
        </header>
        <span>
          {{ todo.description }}
        </span>
      </article>
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

.remove-margin {
  margin: 0;
}

main {
  padding: 0 var(--block-spacing-horizontal);
}


article {
  margin-top: 0;

  h1 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
}

.position-relative {
  display: flex;
  justify-content: space-between;
  gap: 10px;
};


.float-right {
  display: flex;
  gap: 10px;
  align-items: center;

  button {
    margin: 0;
  }
}
</style>