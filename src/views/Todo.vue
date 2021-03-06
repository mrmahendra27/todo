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
    <v-list flat class="pt-2" v-if="todoTasks.length > 0">
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
                <v-icon
                  :disabled="task.done"
                  @click="editTodo(task.id)"
                  color="primary lighten-1"
                  >mdi-pencil</v-icon
                >
              </v-btn>

              <v-btn icon>
                <v-icon @click="deleteDailog(task.id)" color="primary lighten-1"
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
    <!-- Edit Dailog -->
    <v-dialog v-model="editDailog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="text-h5">Edit Task</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-text-field
                outlined
                label="Edit Task"
                v-model="editTask"
                clearable
                hide-details
              ></v-text-field>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="editDailog = false">
            Close
          </v-btn>
          <v-btn color="blue darken-1" text @click="updateTask(taskId)">
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- End Edit Dailog -->
    <!-- Delete Dailog -->
    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="text-h5"> Delete </v-card-title>

        <v-card-text> are you sure you want to delete? </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="dialog = false">
            No
          </v-btn>

          <v-btn color="green darken-1" text @click="deleteTask(taskId)">
            Yes
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- End Delete Dailog -->
    <!-- Message Snackbar -->
    <v-snackbar
      v-model="showMessage"
      rounded="pill"
      :color="messageColor"
      :timeout="timeout"
    >
      {{ message }}

      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="showMessage = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      timeout: 2000,
      showMessage: false,
      message: "",
      messageColor: "green",
      editDailog: false,
      dialog: false,
      task: "",
      editTask: "",
      taskId: "",
      todoTasks: [],
    };
  },
  mounted() {
    if (localStorage.getItem("todoTasks")) {
      try {
        this.todoTasks = JSON.parse(localStorage.getItem("todoTasks"));
      } catch (e) {
        localStorage.removeItem("todoTasks");
      }
    }
  },
  watch: {
    todoTasks: function () {
      localStorage.setItem("todoTasks", JSON.stringify(this.todoTasks));
    },
  },
  methods: {
    /**
     * Add new task
     */
    addNewTask() {
      if (!this.task) {
        this.message = "Please enter task!";
        this.showMessage = true;
        this.messageColor = "red";
        return;
      }

      this.todoTasks.push({
        id: Math.random(5),
        title: this.task,
        done: false,
      });

      this.task = "";

      localStorage.setItem("todoTasks", JSON.stringify(this.todoTasks));

      this.message = "Task added successfully!";
      this.showMessage = true;
    },
    /**
     * Show Edit form
     */
    editTodo(id) {
      this.editDailog = true;

      let task = this.todoTasks.filter((task) => task.id === id)[0];
      this.editTask = task.title;
      this.taskId = task.id;
    },
    /**
     * update Task
     */
    updateTask(id) {
      let task = this.todoTasks.filter((task) => task.id === id)[0];
      task.title = this.editTask;

      this.editTask = "";
      this.editDailog = false;

      this.message = "Task updated successfully!";
      this.showMessage = true;
    },
    /**
     * Toogle task
     */
    toggleTask(id) {
      let updateTask = this.todoTasks.filter((task) => task.id === id)[0];
      updateTask.done = !updateTask.done;
      this.message = updateTask.done
        ? "Task moved to done"
        : "Task removed from done";
      this.showMessage = true;
      this.messageColor = updateTask.done ? "green" : "red";
      localStorage.setItem("todoTasks", JSON.stringify(this.todoTasks));
    },
    /**
     * Open Delete Dailog
     */
    deleteDailog(id) {
      this.dialog = true;

      let task = this.todoTasks.filter((task) => task.id === id)[0];
      this.taskId = task.id;
    },
    /**
     * Delete Task
     */
    deleteTask(id) {
      this.todoTasks = this.todoTasks.filter((task) => task.id !== id);
      this.dialog = false;
      this.message = "Task deleted successfully!";
      this.showMessage = true;
      this.messageColor = "red";
    },
  },
};
</script>
