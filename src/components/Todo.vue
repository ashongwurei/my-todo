<template>
  <div class="container" style="max-width: 550px">
    <!-- HEADING -->
    <h2 class="text-center display-2 mt-5 mb-3">Todo List</h2>

    <!-- INPUT -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter a task"
        class="form-control rounded-0"
      />
      <button @click="addTask" class="btn btn-warning rounded-0">ADD</button>
    </div>

    <!-- TASK TABLE -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{ task.name }}</td>
          <td style="width: 120px">
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
</template>

<script>
export default {
  name: "TodoList",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["Pending", "In-progress", "Completed"],

      tasks: [
        {
          name: `Do Nanle's Assignment`,
          status: "Completed",
        },
        {
          name: "Proofread my Thesis Report",
          status: "In-progress",
        },
        {
          name: "Design the UI for Modish Concept",
          status: "Pending",
        },
      ],
    };
  },

  methods: {
    /* ADD/UPDATE TASK */
    addTask() {
      /* Warinig for Empty Input Field */
      if (!this.task) {
        alert("Enter a Task");
        return;
      }

      if (this.tasks.length === 0) return;

      /* Update Task */
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "Pending",
        });
      } else {
        /* Add New Task */
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },

    /* DELETE TASK */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /* EDIT TASK */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    /* CHANGE TASK STATUS */
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
</style>
