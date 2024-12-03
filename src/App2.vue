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
  <!-- <a v-bind:href="link">Click for Google</a> -->
  <a :href="link">Click for Google</a>
  <button v-on:click="toggle">Change status</button>
</template>
