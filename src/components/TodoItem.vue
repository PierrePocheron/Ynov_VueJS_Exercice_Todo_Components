<template>
  <li class="todo">
    <div class="view">
      <!-- If status = checked add property checked -->
      <input
        type="checkbox"
        class="toggle"
        :checked="task.status"
        @click="updateTaskStatus(task.id, !task.status)"
      />
      <label>{{ task.name }}</label>
      <button class="destroy" @click="removeTask(task.id)"></button>
    </div>
    <input type="text" class="edit" value="{{ task.name }}" />
  </li>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      required: true,
    },
  },

  methods: {
    removeTask() {
      this.$emit("removeTask-request", this.task);
    },
  },
};
</script>
