<template>
  <div>
    <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <input
          autofocus="autofocus"
          autocomplete="off"
          placeholder="What needs to be done?"
          class="new-todo"
          v-model="nameTask"
          @keyup.enter="addTask()"
        />
      </header>

      <section class="main">
        <input
          id="toggle-all"
          type="checkbox"
          class="toggle-all"
          v-model="inputToggleAll"
          @change="updateAllTaskStatus()"
        />
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list">
          <TodoItem
            v-for="task in filteredTaskList"
            :key="task.id"
            :taskAttr="task"
          />
        </ul>
      </section>

      <footer class="footer">
        <span class="todo-count">
          <strong> {{ counterTaskLeft }} </strong> item left
        </span>
        <ul class="filters">
          <li>
            <a
              href="#"
              @click.prevent="filter = 'all'"
              :class="{ selected: filter === 'all' }"
              >All</a
            >
          </li>
          <li>
            <a
              href="#"
              @click.prevent="filter = 'active'"
              :class="{ selected: filter === 'active' }"
              >Active</a
            >
          </li>
          <li>
            <a
              href="#"
              @click.prevent="filter = 'completed'"
              :class="{ selected: filter === 'completed' }"
              >Completed</a
            >
          </li>
        </ul>
        <button class="clear-completed" @click="removeTaskCompleted()">
          Clear completed
        </button>
      </footer>
    </section>
    <footer class="info">
      <p>Double-click to edit a todo</p>
    </footer>
  </div>
</template>

<script>
import TodoItem from "@/components/TodoItem.vue";
export default {
  components: {
    TodoItem,
  },
  data() {
    return {
      taskList: [
        { id: 1, name: "Task 1", status: true },
        { id: 2, name: "Task 2", status: true },
        { id: 3, name: "Task 3", status: false },
      ],
      nextId: 4,
      nameTask: "",
      inputToggleAll: false,
      filter: "all",
    };
  },

  methods: {
    addTask() {
      this.taskList.push({
        id: this.nextId,
        name: this.nameTask,
        status: false,
      });
      this.nextId++;
      this.nameTask = "";
    },

    removeTask(id) {
      this.taskList = this.taskList.filter((task) => task.id !== id);
    },

    removeTaskCompleted() {
      this.taskList = this.taskList.filter((task) => !task.status);
    },

    updateTaskStatus(id, status) {
      this.taskList = this.taskList.map((task) => {
        if (task.id === id) {
          task.status = status;
        }
        return task;
      });
    },

    updateAllTaskStatus() {
      this.taskList.forEach((task) => {
        task.status = this.inputToggleAll;
      });
    },
  },

  computed: {
    counterTaskLeft: function () {
      var tasksLeft = this.taskList.filter((task) => !task.status);
      return tasksLeft.length;
    },

    filteredTaskList() {
      if (this.filter === "active") {
        return this.taskList.filter((task) => !task.status);
      } else if (this.filter === "completed") {
        return this.taskList.filter((task) => task.status);
      } else return this.taskList;
    },
  },

  setup() {},
};
</script>
