<template>
  <div class="container">
    <div>
      <input
        v-model="task"
        type="text"
        placeholder="enter the task"
        class="form-control"
      />
      <button
        @click="submitTask"
        class="btn btn-dark mt-2"
        :disabled="isDisabled"
      >
        <span class="fas fa-plus"></span>
      </button>
    </div>

    <H1 class="mt-3">TODO List</H1>

    <table class="table table-hover table-bordered mt-3">
      <thead>
        <th>Task</th>
        <th>Status</th>
        <th>Update</th>
        <th>Delete</th>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{ task.name }}</td>
          <td>{{ task.status }}</td>
          <td>
            <div @click="editTask(task.id)" class="pointer">
              <span class="fas fa-pen-fancy"></span>
            </div>
          </td>
          <td>
            <div @click="deleteTask(task.id)" class="pointer">
              <span class="fas fa-trash-alt"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import firebase from "@/firebaseApi.js";
export default {
  name: "InputComponent",
  data() {
    return {
      task: "",
      tasks: [
        // {
        //   name: "Do work of the day",
        //   status: "to-do",
        // },
        // {
        //   name: "Finish the task before 5",
        //   status: "to-do",
        // },
      ],
    };
  },
  computed: {
    isDisabled() {
      return this.task.length == 0;
    },
  },
  methods: {
    submitTask() {
      // if(this.task.length === 0){
      //     alert("please enter the task");
      // }
      // this.tasks.push({
      //     name: this.task,
      //     status: "to-do"
      // })
      // this.task= '';
      const todoRef = firebase.database().ref("Todo");
      const task = {
        name: this.task,
        status: "to-do",
      };
      todoRef.push(task);
      this.task = "";
    },
    editTask(id) {
      //   this.task = this.tasks[index].name;
      const todoRef = firebase.database().ref("Todo").child(id);
      todoRef.update({
        name: this.task,
      });
      this.task = "";
    },
    deleteTask(id) {
      //   this.tasks.splice(index, 1);
      const todoRef = firebase.database().ref("Todo").child(id);
      todoRef.remove();
    },
    getTodos() {
      const todoRef = firebase.database().ref("Todo");
      var vm = this;
      todoRef.on("value", (snapshot) => {
        const todos = snapshot.val();
        vm.tasks = [];
        for (let id in todos) {
          vm.tasks.push({
            name: todos[id].name,
            status: todos[id].status,
            id: id,
          });
        }
      });
    },
  },
  mounted() {
    // const db = firebase.database().ref();

    // console.warn(db);
    this.getTodos();
  },
};
</script>

<style>
.pointer {
  cursor: pointer;
}
</style>