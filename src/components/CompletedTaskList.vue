<template>
    <div v-if="tasks.length">
        <h2 class="mt-5">Completed Items</h2>
        <ul class="list-group">
            <li v-for="(task, index) in tasks" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
                <div>{{ task.name }}</div>
                <div class="d-flex align-items-center">
                    <div class="me-2"><i>Completed: {{ task.completedAt }}</i></div>
                    <button @click="deleteCompletedTask(index)" class="btn btn-danger btn-sm"><i class="fas fa-trash-alt"></i></button>
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
    methods: {
        deleteCompletedTask(index) {
            this.$emit("deleteCompletedTask", index);
        },
    },
};
</script>