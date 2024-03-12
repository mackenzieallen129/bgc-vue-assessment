<template>
    <div>
        <ul class="list-group">
            <li v-for="(task, index) in tasks" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
                <div class="flex-grow-1">
                    <div v-if="index !== editingIndex">
                        <div class="form-check">
                        <input class="form-check-input me-3" type="checkbox" v-model="task.completed" @change="completeTask(index)">
                        <label class="form-check-label">{{ task.name }}</label>
                        </div>
                    </div>
                    <div v-else>
                        <input v-model="editedTask" @keyup.enter="saveTask(index)" @focusout="saveTask(index)" class="form-control" autofocus>
                    </div>
                </div>
                <div v-if="index !== editingIndex" class="btn-group" role="group">
                    <button @click="startEditing(index)" class="btn btn-secondary btn-sm"><i class="fas fa-edit"></i></button>
                    <button @click="deleteTask(index)" class="btn btn-danger btn-sm"><i class="fas fa-trash-alt"></i></button>
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
            this.editedTask = this.tasks[index].name;
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
            this.editedTask = this.tasks[index].name;
            this.editingIndex = null;
        },
        deleteTask(index) {
            this.$emit("taskDeleted", index);
        },
        completeTask(index) {
            this.$emit("taskCompleted", index);
        },
    },
};
</script>