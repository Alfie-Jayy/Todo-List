<template>
  <div>
    <h1 class="text-center text-white bg-primary p-3" style="width: 100%">
      {{ name }}'s todo List
    </h1>

    <div class="m-auto p-3" style="max-width: 400px">
      <div class="row my-5">
        <div class="col">
          <input
            @keyup.enter="addTaskBtn"
            v-model="addTask"
            type="text"
            class="form-control"
          />
        </div>
        <div class="col">
          <button @click="addTaskBtn" class="btn btn sm btn-secondary">
            Add
          </button>
        </div>
      </div>

      <div class="" v-if="filterTasks.length > 0">
        <div class="row mb-4">
          <div class="col fs-2">Tasks</div>
          <div class="col-3 fs-2">Status</div>
        </div>

        <div class="row mb-3" v-for="(task, index) in filterTasks" :key="index">
          <div class="col" v-bind:class="{ delete: task.status }">
            {{ task.action }}
          </div>
          <div class="col-3">
            <input type="checkbox" v-model="task.status" />
          </div>
        </div>
      </div>

      <div class="alert alert-warning text-center my-4" v-else>
        Here is no tasks!
      </div>

      <div class="d-flex mt-5">
        <div class="me-3">
          <button @click="toggleBtn" class="btn btn-sm btn-danger">
            {{ showAllTasks ? "Hide Completed Tasks" : "Show All Tasks" }}
          </button>
        </div>

        <div class="">
          <button class="btn btn-sm btn-warning" @click="deleteTaskBtn()">Delete Tasks</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    name: "CODE LAB",
    addTask: "",
    tasks: [],
    showAllTasks: true,
  }),
  computed: {
    filterTasks() {
      return this.showAllTasks
        ? this.tasks
        : this.tasks.filter((t) => !t.status);
    },
  },
  methods: {
    toggleBtn() {
      this.showAllTasks = !this.showAllTasks;
    },
    addTaskBtn() {
      if (this.addTask === "") {
        alert("You need to write something to add at Task list!");
      } else {
        this.tasks.push({
          action: this.addTask,
          status: false,
        });
        this.storeData()
        this.addTask = "";
      }
    },
    deleteTaskBtn(){
      this.tasks = this.tasks.filter((t)=>!t.status);
      this.storeData()
    },
    storeData(){
      localStorage.setItem("myLocalTask", JSON.stringify(this.tasks))
    }
  },
  mounted() {
    let data = localStorage.getItem("myLocalTask");
    if (data !== null) {
      this.tasks = JSON.parse(data);
    }
  },
};
</script>

<style>
.delete {
  text-decoration: line-through;
}
</style>
