<template>
    <div>
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
    </div>
</template>

<script>
export default {
    name: "EditTask",
    props: {
        id: Number,
        todoTasks: Object
    },
    data() {
        return {
            task: ''
        }
    },
    methods: {
        editTask(id) {

        }
    },
}
</script>