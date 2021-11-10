<script setup lang="ts">
import { version, ref } from 'vue'
</script>

<template>
  <header>
    <hgroup>
      <h1>Basic Todo App</h1>
      <small>A basic Todo App build with Vue {{ version }}</small>
    </hgroup>
  </header>
  <main>
    <form>
      <label for="todo-input">Enter a new Todo</label>
      <input
        id="todo-input"
        type="text"
        name="todo-input"
        placeholder="Take out the trash"
        required
      >
      <label for="todo-type">Select a type:</label>
      <select
        id="todo-type"
        multiple="true"
      >
        <option>General</option>
        <option>Shoping</option>
        <option>Misc.</option>
      </select>
      <button type="submit">
        Save Todo
      </button>
    </form>
    <hr>
    <section>
      <table role="grid">
        <thead>
          <tr>
            <th scope="col">
              #
            </th>
            <th scope="col">
              Heading
            </th>
            <th scope="col">
              Heading
            </th>
            <th scope="col">
              Actions
            </th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th scope="row">
              1
            </th>
            <td>Cell</td>
            <td>Cell</td>
            <th scope="row">
              Actions
            </th>
          </tr>
          <tr>
            <th scope="row">
              2
            </th>
            <td>Cell</td>
            <td>Cell</td>
            <th scope="row">
              Actions
            </th>
          </tr>
          <tr>
            <th scope="row">
              3
            </th>
            <td>Cell</td>
            <td>Cell</td>
            <th scope="row">
              Actions
            </th>
          </tr>
        </tbody>
      </table>
    </section>
  </main>
</template>