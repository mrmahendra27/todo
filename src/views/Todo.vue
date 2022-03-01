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
                <v-icon @click="editDailog = true" color="primary lighten-1"
                  >mdi-pencil</v-icon
                >
              </v-btn>
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
                          label="edit Task"
                          clearable
                          hide-details
                        ></v-text-field>
                      </v-row>
                    </v-container>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="editDailog = false"
                    >
                      Close
                    </v-btn>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="editDailog = false"
                    >
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              
              <v-btn icon>
                <v-icon @click="dialog = true" color="primary lighten-1"
                  >mdi-delete</v-icon
                >
              </v-btn>

              <v-dialog v-model="dialog" max-width="290">
                <v-card>
                  <v-card-title class="text-h5"> Delete </v-card-title>

                  <v-card-text> are you sure you want to delete? </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>

                    <v-btn color="green darken-1" text @click="dialog = false">
                      No
                    </v-btn>

                    <v-btn
                      color="green darken-1"
                      text
                      @click="deleteTask(task.id)"
                    >
                      Yes
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
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
      editDailog: false,
      dialog: false,
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
      this.dialog = false;
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
