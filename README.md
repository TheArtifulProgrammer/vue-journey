# Vue.js Learning Reference

This document provides an overview of concepts and practical examples for the Vue.js Options API and Composition API. Use this as a reference for your learning progress.

---

## Options API Example

### JavaScript Section

```vue
<script>
export default {
  data() {
    return {
      name: "John Smith",
      status: "pending",
      age: 30,
      job: "Software Engineer",
      link: "http://www.google.com",
      tasks: [
        { id: 1, name: "Mbudzi" },
        { id: 2, name: "Cow" },
        { id: 3, name: "Dog" },
        { id: 4, name: "Cat" },
      ],
    };
  },
  methods: {
    toggle() {
      this.status = this.status === "active" ? "pending" : "active";
    },
  },
};
</script>
```

### Template Section

```vue
<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>
  <p>Age: {{ age }}</p>

  <h3>Tasks</h3>
  <ul v-for="task in tasks" :key="task.id">
    <li>{{ task.name }}</li>
  </ul>

  <a :href="link">Click for Google</a>
  <button v-on:click="toggle">Change status</button>
</template>
```

---

## Composition API Example

### JavaScript Section

```vue
<script setup>
import { ref, onMounted } from "vue";

const name = ref("John Smith");
const status = ref("active");
const age = ref(24);
const tasks = ref([
  { id: 1, name: "Mbudzi" },
  { id: 2, name: "Cow" },
  { id: 3, name: "Dog" },
  { id: 4, name: "Cat" },
]);
const newTask = ref("");

const toggle = () => {
  status.value = status.value === "active" ? "pending" : "active";
};

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({
      id: tasks.value.length + 1,
      name: newTask.value.trim(),
    });
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => ({ id: task.id, name: task.title }));
  } catch (error) {
    console.log(error);
  }
});
</script>
```

### Template Section

```vue
<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>
  <p>Age: {{ age }}</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add a task:</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <input type="submit" value="Submit" />
  </form>

  <h3>Tasks</h3>
  <ul v-for="(task, index) in tasks" :key="task.id">
    <li>
      <span>{{ task.name }}</span>
      <button @click="deleteTask(index)">Delete</button>
    </li>
  </ul>

  <button @click="toggle">Change status</button>
</template>
```

---

## Concepts Covered

### Options API

- **`data`**: Defines reactive state.
- **`methods`**: Holds functions that interact with the component's state or DOM.
- **Directives**: Includes `v-if`, `v-else-if`, `v-for`, `v-bind`, and `v-on`.

### Composition API

- **`ref`**: Creates a reactive reference.
- **`onMounted`**: Lifecycle hook for when the component is mounted.
- **Reactive Methods**:
  - Adding a new task with `addTask()`.
  - Deleting a task with `deleteTask()`.
  - Toggling the status with `toggle()`.
- **Fetching Data**:
  - Uses `fetch` API within `onMounted` to populate tasks dynamically.

### Additional Notes

- Use **`v-model`** for two-way data binding in forms.
- **Event Binding**: Simplified event binding syntax with `@` (e.g., `@click`, `@submit.prevent`).

---
