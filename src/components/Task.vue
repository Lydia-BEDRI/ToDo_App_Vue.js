<script setup>
const props = defineProps({
  task: {
    type: Object,
    required: true
  }
});

</script>

<template>
      <div class="task">
        <h3>
          {{ task.name }}
        </h3>
        <p>
          {{ task.description }}
        </p>
        <div class="task-check">
          <input type="checkbox" :checked="task.completed" />
          <label>
            Done
          </label>
        </div>
      </div>

</template>

<style scoped>
  
</style>