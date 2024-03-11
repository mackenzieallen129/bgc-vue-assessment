<template>
  <div>
    <ul class="list-group">
      <li v-for="(task, index) in tasks" :key="index" class="list-group-item">{{ task }}</li>
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
};
</script>