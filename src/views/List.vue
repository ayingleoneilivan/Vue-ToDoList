<template>
  <div class="home">
    <v-text-field
      v-model="newTaskTitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-3"
      outlined
      label="Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable>
    </v-text-field>

    <v-list class="pt-0" flat>
      <div v-for="task in tasks" :key="task.id">
        <v-list-item :class="{'blue-grey lighten-3': task.done}" class="col-1">
          <template v-slot:default>
            <v-list-item-action v-if="!task.editing">
              <v-checkbox
                :input-value="task.done"
                @click.stop="doneTask(task.id)"
                color="blue-grey darken-1">
              </v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{'text-decoration-line-through': task.done}">
                <v-text-field
                  v-if="task.editing"
                  v-model="task.title"
                  hide-details>
                </v-text-field>
                <span v-else @dblclick="task.editing = true">{{ task.title }}</span>
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn 
                @click.stop="deleteTask(task.id)"
                icon>
                <v-icon color="blue-grey darken-1">mdi-delete</v-icon>
              </v-btn>

              <v-btn 
                @click.stop="task.editing = true"
                v-if="!task.editing"
                icon>
                <v-icon color="blue-grey darken-1">mdi-pencil</v-icon>
              </v-btn>

              <v-btn 
                @click.stop="editTask(task)"
                v-if="task.editing"
                icon>
                <v-icon color="blue-grey darken-1">mdi-check</v-icon>
              </v-btn>

            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
  </div>
</template>

<script>
export default {
  name: 'Home',

  data() {
    return {
      newTaskTitle: '',
      tasks: []
    }
  },

  methods: {
    addTask() {
      if (this.newTaskTitle.trim() === '') return; // Prevent adding empty tasks

      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false,
        editing: false
      }
      this.tasks.push(newTask)
      this.newTaskTitle = '';  // Clear the input after adding the task
    },
    
    doneTask(id) {
      let task = this.tasks.find(task => task.id == id)
      task.done = !task.done
    },
    
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    },

    editTask(task) {
      task.editing = !task.editing;
    }
  }
}
</script>
