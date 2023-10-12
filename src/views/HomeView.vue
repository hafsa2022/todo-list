<template>
  <v-card
    class="mx-auto"
    style="margin-top: 200px"
    width="500"
    height="auto"
    elevation="4"
  >
    <v-card-title class="text-center mb-6"><b>Todo List</b></v-card-title>
    <v-row class="mx-4">
      <v-col clos="9">
        <label for="name">Task Name</label>
        <v-text-field
          id="name"
          variant="outlined"
          placeholder="Enter Task Name"
          v-model="taskName"
          color="#5f3a81"
          @keydown.enter="addTask"
        >
          <template v-slot:append-inner>
            <v-fade-transition>
              <v-btn
                v-show="taskName"
                icon="mdi-plus-circle"
                variant="text"
                @click="addTask"
              ></v-btn>
            </v-fade-transition> </template></v-text-field></v-col
    ></v-row>
    <v-row class="mx-4 mb-6" justify="center">
      <v-col cols="12">
        <v-card elevation="0">
          <v-row class="mt-4 mx-auto">
            <v-col cols="4"
              ><h4>Tasks :&nbsp; {{ tasks.length }}</h4></v-col
            >
            <v-col cols="6"
              ><h4>Completed Tasks :&nbsp; {{ completedTasks }}</h4></v-col
            >
            <v-col cols="2" v-if="tasks.length > 0">
              <v-progress-circular
                v-model="progress"
                class="me-2"
              ></v-progress-circular
            ></v-col>
            <v-divider class="mb-4"></v-divider>
          </v-row>
          <v-row v-if="tasks.length > 0">
            <v-col cols="12">
              <v-template v-for="(task, i) in tasks" :key="i">
                <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider
                ><v-list-item @click="setCompletedTasks(task)">
                  <template v-slot:prepend>
                    <v-checkbox-btn
                      v-model="task.taskComplete"
                      color="grey"
                    ></v-checkbox-btn>
                  </template>

                  <v-list-item-title>
                    <span
                      :class="task.taskComplete ? 'text-grey' : 'text-black'"
                      >{{ task.taskName }}</span
                    >
                  </v-list-item-title>

                  <template v-slot:append>
                    <v-expand-x-transition>
                      <v-icon
                        v-if="task.taskComplete"
                        color="error"
                        @click="deleteTask(task)"
                      >
                        mdi-delete
                      </v-icon>
                    </v-expand-x-transition>
                  </template>
                </v-list-item></v-template
              >
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-card>
</template>

<script>
export default {
  name: "HomeView",
  data() {
    return {
      taskName: "",
      tasks: [],
      tasksDone: [],
      taskDone: false,
    };
  },
  mounted() {
    this.tasks = JSON.parse(localStorage.getItem("tasks"));
  },
  components: {},
  computed: {
    progress() {
      return (this.completedTasks / this.tasks.length) * 100;
    },
    completedTasks() {
      return this.tasks.filter((task) => task.taskComplete).length;
    },
  },
  methods: {
    addTask() {
      this.tasks.push({ taskName: this.taskName, taskComplete: this.taskDone });
      localStorage.setItem("tasks", JSON.stringify(this.tasks || {}));
    },
    deleteTask(task) {
      const taskIndex = this.tasks.indexOf(task);
      this.tasks.splice(taskIndex, 1);
      console.log("taskIndex", taskIndex);
      // localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    setCompletedTasks(CompletedTask) {
      const CompletedTaskIndex = this.tasks.indexOf(CompletedTask);
      this.tasks.forEach((task) => {
        const taskIndex = this.tasks.indexOf(task);
        if (CompletedTaskIndex === taskIndex) {
          this.tasks[CompletedTaskIndex].taskComplete =
            !this.tasks[CompletedTaskIndex].taskComplete;
        }
      });
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
  },
};
</script>
<style>
.logo {
  background-color: #aa9bb8;
}
</style>
