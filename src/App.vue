<template>
  <div class="container mt-5">
    <h1 class="mb-4">To-Do List</h1>
    <add-task @taskAdded="addTask"></add-task>
    <task-list :tasks="tasks"></task-list>
  </div>
</template>

<script>
import AddTask from "./components/AddTask.vue";
import TaskList from "./components/TaskList.vue";

export default {
  name: "App",
  components: {
    AddTask,
    TaskList,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    addTask(task) {
      this.tasks.push(task);
    },
  },
};
</script>