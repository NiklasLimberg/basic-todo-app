
<template>
  <header class="hgroup">
    <h1>Basic Todo App</h1>
    <small>A basic Todo App build with Vue {{ version }}</small>
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
      />
      <textarea
        id="todo-textarea"
        v-model="todoDraft.description"
        name="todo-textarea"
        placeholder="Take out the trash"
        required
      />
      <button type="submit">Save Todo</button>
    </form>
    <hr />
    <section>
      <div class="todo-status-container">
        <div>
          <label for="todo-filter">Filter:</label>
          <select id="todo-filter" v-model="filter">
            <option value="all">All</option>
            <option value="open">Open</option>
            <option value="closed">Closed</option>
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

<script lang="ts">
import { defineComponent, version } from 'vue'
import { v4 as uuid } from 'uuid';

import { Todo } from './interfaces/todo';
import TodoCard from './components/TodoCard.vue';

enum Filter {
  All = 'all',
  Open = 'open',
  Closed = 'closed'
}


export default defineComponent({
  components: {
    TodoCard
  },
  data() {
    return {
      todoDraft: {
        id: uuid(),
        title: '',
        description: '',
        createdAt: new Date(),
        closed: false
      } as Todo,
      todos: [{
        id: uuid(),
        title: 'Test',
        description: 'Test description',
        closed: false,
        createdAt: new Date()
      }] as Todo[],
      filter: 'all' as Filter,
      version: version
    }
  },
  methods: {
    saveTodo(event: Event) {
      event.preventDefault();

      this.todos.push(this.todoDraft);

      this.todoDraft = {
        id: uuid(),
        title: '',
        description: '',
        createdAt: new Date(),
        closed: false
      }
    },
    changeStatus(id: string) {
      const todo = this.todos.find(todo => todo.id === id);
      if (!todo) {
        throw new Error(`Unable to fin todo with id: ${id}`);
      }
      todo.closed = !todo.closed;
    }, deleteTodo(id: string) {
      const index = this.todos.findIndex(todo => todo.id === id);
      this.todos.splice(index, 1);
    }
  },
  computed: {
    filteredTodos(): Todo[] {
      if (this.filter === Filter.All) {
        return this.todos;
      }

      const filterClosed = this.filter === Filter.Closed;

      return this.todos.filter(todo => todo.closed === filterClosed);
    }
  }
})
</script>

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

.hgroup {
  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    border-left: 0.25ch solid var(--primary-hover);
    padding-left: 0.25ch;
    margin-bottom: 0px;
  }
}

main {
  padding: 0;
}
</style>