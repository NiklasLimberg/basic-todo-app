<template>
  <article>
    <header class="position-relative">
      <h1 class="remove-margin">{{ todo.title }}</h1>
      <div class="float-right">
        <button @click="$emit('changeStatus', todo.id)">{{ todo.closed ? 'Open' : 'Close' }}</button>
        <button @click="$emit('delete', todo.id)">Delete</button>
      </div>
    </header>
    <span>{{ todo.description }}</span>
  </article>
</template>


<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { Todo } from '../interfaces/todo';


export default defineComponent({
  emits: {
    delete(payload: string) {
      // perform runtime validation
      return typeof payload === 'string'
    },
    changeStatus(payload: string) {
      return typeof payload === 'string'
    },
  },
  props: {
    todo: {
      type: Object as PropType<Todo>,
      required: true
    }
  },
})
</script>

<style lang="scss" scoped>
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
}

.remove-margin {
  margin: 0;
}

.float-right {
  display: flex;
  gap: 10px;
  align-items: center;

  button {
    margin: 0;
  }
}
</style>