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
  <!-- <a v-bind:href="link">Click for Google</a> -->
  <!-- <a :href="link">Click for Google</a> -->
  <!-- <button v-on:click="toggle">Change status</button> -->
  <button @click="toggle">Change status</button>
</template>
