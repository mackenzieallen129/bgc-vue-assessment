<template>
  <div class="container mt-5">
    <h2 class="mb-4">To-Do Items</h2>
    <add-task @taskAdded="addTask"></add-task>
    <task-list :tasks="tasks" @taskEdited="editTask" @taskDeleted="deleteTask" @taskCompleted="completeTask"></task-list>
    <completed-task-list :tasks="completedTasks" @deleteCompletedTask="deleteCompletedTask"></completed-task-list>
  </div>
</template>

<script>
import AddTask from "./components/AddTask.vue";
import TaskList from "./components/TaskList.vue";
import CompletedTaskList from "./components/CompletedTaskList.vue";

export default {
  name: "App",
  components: {
    AddTask,
    TaskList,
    CompletedTaskList,
  },
  data() {
    return {
      newTask: "",
      tasks: [],
      completedTasks: [],
    };
  },
  methods: {
    addTask(task) {
      this.tasks.push({ name: task, completed: false });
    },
    editTask({ index, task }) {
      this.tasks[index].name = task;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    completeTask(index) {
      const completedTask = this.tasks.splice(index, 1)[0];
      const now = new Date();
      const hours = now.getHours();
      const minutes = String(now.getMinutes()).padStart(2, "0");
      const ampm = hours >= 12 ? "PM" : "AM";
      const completedAt = `${now.toLocaleDateString()} at ${hours % 12 || 12}:${minutes} ${ampm}`;
      completedTask.completedAt = completedAt;
      this.completedTasks.push(completedTask);
    },
    deleteCompletedTask(index) {
      this.completedTasks.splice(index, 1);
    },
  },
};
</script>