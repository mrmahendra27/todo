<template>
  <div class="home pa-6">
    <v-text-field
      outlined
      label="Add Task"
      append-icon="mdi-plus"
      clearable
      v-model="task"
      hide-details
      @click:append="addNewTask"
      @keyup.enter="addNewTask"
    ></v-text-field>
    <v-list flat class="pt-0" v-if="todoTasks.length">
      <div v-for="task in todoTasks" :key="task.id">
        <v-list-item
          @click="toggleTask(task.id)"
          :class="{ 'blue lighten-5': task.done }"
        >
          <template v-slot:default="{ active }">
            <v-list-item-action>
              <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
                >{{ task.title }}</v-list-item-title
              >
            </v-list-item-content>

            <v-list-item-action>
              <v-btn icon>
                <v-icon @click="deleteTask(task.id)" color="primary lighten-1"
                  >mdi-delete</v-icon
                >
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
    <div class="text-center pt-10" v-else>
      <strong>No Task Yet...</strong>
    </div>
  </div>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      task: "",
      todoTasks: [],
    };
  },
  methods: {
    toggleTask(id) {
      let updateTask = this.todoTasks.filter((task) => task.id === id)[0];
      updateTask.done = !updateTask.done;
    },
    deleteTask(id) {
      this.todoTasks = this.todoTasks.filter((task) => task.id !== id);
    },
    addNewTask() {
      this.todoTasks.push({
        id: Math.random(5),
        title: this.task,
        done: false,
      });

      this.task = "";

      localStorage.setItem("todoTasks", this.todoTasks);
    },
  },
};
</script>
