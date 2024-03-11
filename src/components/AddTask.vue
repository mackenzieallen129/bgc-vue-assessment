<template>
  <div class="input-group mb-3">
    <input type="text" v-model="newTask" class="form-control" placeholder="Add a new task">
    <button @click="addTask" class="btn btn-primary">Add</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.$emit("taskAdded", this.newTask);
        this.newTask = "";
      }
    },
  },
};
</script>