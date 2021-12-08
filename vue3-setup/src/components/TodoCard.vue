<script setup lang="ts">
import { Todo } from '../interfaces/todo';

import { defineProps, defineEmits } from 'vue'

const props = defineProps<{
  todo: Todo
}>()

const emit = defineEmits<{
  (e: 'delete', id: string): void
  (e: 'changeStatus', id: string): void
}>()

</script>

<template>
  <article>
    <header class="position-relative">
      <h1 class="remove-margin">
        {{ todo.title }}
      </h1>
      <div class="float-right">
        <button @click="emit('changeStatus', props.todo.id)">
          {{ todo.closed ? 'Open' : 'Close' }}
        </button>
        <button @click="emit('delete', props.todo.id)">
          Delete
        </button>
      </div>
    </header>
    <span>
      {{ props.todo.description }}
    </span>
  </article>
</template>

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
};

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