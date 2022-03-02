<!-- PROJECT STRUCTURE -->
<template>
  <div class="container d-flex justify-content-center align-items-center">
    <div class="content">
      <!-- Heading -->
      <h1 class="text-center display-1 mb-5">My Todo</h1>

      <!-- Input -->
      <div class="d-flex">
        <input
          v-model="task"
          type="text"
          placeholder="Enter a task"
          class="form-control rounded-0"
        />
        <button @click="addTask" class="btn btn-warning rounded-0">ADD</button>
      </div>

      <!-- Task Table -->
      <table class="table table-bordered mt-5">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col" class="text-center">
              Edit
            </th>
            <th scope="col" class="text-center">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in offlineTasks" :key="index">
            <td>{{ task.name }}</td>
            <td
              style="width: 120px"
              data-toggle="tooltip"
              data-placement="right"
              title="Click to change the status of the task"
            >
              <span
                @click="changeStatus(index)"
                class="pointer"
                :class="{
                  'text-danger': task.status === 'Pending',
                  'text-warning': task.status === 'In-progress',
                  'text-success': task.status === 'Completed',
                }"
              >
                {{ task.status }}
              </span>
            </td>
            <td>
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen pointer"></span>
              </div>
            </td>
            <td>
              <div class="text-center" @click="deleteTask(index)">
                <span class="fa fa-trash pointer"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<!-- PROJECT LOGIC -->
<script>
export default {
  name: "TodoList",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      offlineTasks: "",
      editedTask: null,
      availableStatuses: ["Pending", "In-progress", "Completed"],
      tasks: [],
    };
  },

  methods: {
    /* Add/Update Task */
    addTask() {
      /* Empty Input Field */
      if (!this.task) {
        alert("Enter a Task");
        return;
      }

      /* Update Task */
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "Pending",
        });
        this.saveTasks();
      } else {
        /* Add New Task */
        this.offlineTasks[this.editedTask].name = this.task;
        this.editedTask = null;
        this.saveTasks();
      }

      this.task = "";
    },

    /* Delete Task */
    deleteTask(index) {
      this.offlineTasks.splice(index, 1);
      this.saveTasks();
    },

    /* Edit Task */
    editTask(index) {
      this.task = this.offlineTasks[index].name;
      this.editedTask = index;
    },

    /* Change Task Status */
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(
        this.offlineTasks[index].status
      );

      if (++newIndex > 2) newIndex = 0;
      this.offlineTasks[index].status = this.availableStatuses[newIndex];
      this.saveTasks();
    },

    saveTasks() {
      let newArray = this.tasks.concat(this.offlineTasks);
      localStorage.setItem("tasks", JSON.stringify(newArray));
      this.getTasks();
      this.tasks = [];
    },

    getTasks() {
      this.offlineTasks = JSON.parse(localStorage.getItem("tasks"));
    },
  },

  created() {
    this.getTasks();
  },
};
</script>

<style>
.container {
  min-width: 100vw;
  min-height: 100vh;
}

.pointer {
  cursor: pointer;
}

.content {
  background: #c3c9ff;
  width: 50vw;
  padding: 40px;
  border-radius: 5px;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
</style>
