<template>
  <div class="container">
    <h2 class="text-center mt-4">My Vue todo App</h2>
    <!-- input -->
    <div class="d-flex">
      <input
        v-model="todo"
        type="text"
        placeholder="Enter todo"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-primary rounded-0">
        Submit
      </button>
    </div>
    <!-- tables -->
    <table
      class="table table-striped table-bordered table-hover mt-5 text-center"
    >
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th colspan="2">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </td>
          <td style="width: 120px">
            <span class="pointer" :class="{'text-danger':task.status === 'to-do','text-warning':task.status === 'in-progress','text-success':task.status === 'finished'}" @click="changeStatus(index)">
              {{ toTitleCase(task.status) }}
              </span
            >
          </td>
          <td>
            <PencilAltIcon
              @click="editTodo(index)"
              class="rounded-circle"
              style="height: 20px"
            />
          </td>
          <td>
            <TrashIcon
              @click="deleteTask(index)"
              class="rounded-circle"
              style="height: 20px"
            />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template> 

<script>
import { PencilAltIcon, TrashIcon } from "@heroicons/vue/solid";
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },
  components: {
    PencilAltIcon,
    TrashIcon,
  },
  data() {
    return {
      todo: "",
      editedTask: null,
      availableStatus: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Go for shopping",
          status: "to-do",
        },
        {
          name: "Do some coding",
          status: "doNe",
        },
        {
          name: "Buy apple juice",
          status: "to-do",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.editedTask === null) {
        if (this.todo.length === 0) return;
        this.tasks.push({ name: this.todo, status: "to-do" });
        this.todo = "";
      } else {
        this.tasks[this.editedTask].name = this.todo;
        this.todo = "";
        this.editedTask = null;
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      console.log(index);
    },
    editTodo(index) {
      console.log(index);
      this.todo = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
    },
    toTitleCase(str) {
      return str.replace(/\w\S*/g, function (txt) {
        // return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase();
        return txt.charAt(0).toUpperCase() + txt.slice(1);
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
