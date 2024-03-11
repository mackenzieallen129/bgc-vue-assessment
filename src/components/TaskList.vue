<template>
  <div>
    <ul class="list-group">
      <li v-for="(task, index) in tasks" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
        <div class="flex-grow-1">
          <div v-if="index !== editingIndex" @click="startEditing(index)">{{ task }}</div>
          <div v-else>
            <input v-model="editedTask" @keyup.enter="saveTask(index)" @focusout="cancelEdit(index)" class="form-control">
          </div>
        </div>
        <div v-if="index !== editingIndex" class="btn-group" role="group">
          <button @click="startEditing(index)" class="btn btn-secondary btn-sm">Edit</button>
          <button @click="deleteTask(index)" class="btn btn-danger btn-sm">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    tasks: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      editingIndex: null,
      editedTask: "",
    };
  },
  methods: {
    startEditing(index) {
      this.editingIndex = index;
      this.editedTask = this.tasks[index];
    },
    saveTask(index) {
      if (this.editedTask.trim() === "") {
        this.cancelEdit(index);
      } else {
        this.$emit("taskEdited", { index, task: this.editedTask });
        this.editingIndex = null;
      }
    },
    cancelEdit(index) {
      this.editedTask = this.tasks[index];
      this.editingIndex = null;
    },
    deleteTask(index) {
      this.$emit("taskDeleted", index);
    },
  },
};
</script>
