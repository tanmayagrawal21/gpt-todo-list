<template>
  <v-container> 
  <h1 class="text-center"> To Do List </h1>
    <!-- each task encapsulated in a box with option to delete -->
    <v-container v-for="(task, index) in tasks" :key="index">
      <!--delete button-->
      <v-btn icon @click="deleteTask(index)">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
      <!--list item-->
      <ListItem :task="task.task" :subtasks="task.subtasks" :class="'list-item'" />
    </v-container>
    

    <!-- Add new list item-->
    <v-container class="list-item">
      <v-container class="list-item__title">
        <v-text-field 
          label="Add new task" 
          hide-details="auto" 
          v-model="newTask"
          @keyup.enter="addTask"
          />
        <v-btn variant="outlined" class="text-center mt-3" @click="addTask">Add</v-btn>
      </v-container>
    </v-container>
  </v-container> 
</template>

<script>
import ListItem from './ListItem.vue'

export default {
  name: 'HelloWorld',
  components: {
    ListItem
  },
  data: () => ({
    tasks: [

    ],
    api_key: null
  }), mounted(){
    this.api_key = localStorage.getItem('api_key')
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
    if (!this.api_key) {
      this.api_key = prompt("Please enter your OpenAI API key")
      localStorage.setItem('api_key', this.api_key.trim())
    }
  },
  methods: {
    addTask() {
      // add data from text field to tasks
      if (this.newTask.trim() == "") {
        return
      }
      this.tasks.push({
        task: this.newTask.trim(),
        subtasks: [
        ]
      })
      // clear the text field
      this.newTask = ""
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    deleteTask(index) {
      this.tasks.splice(index, 1) 
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  }
}
</script>

<style scoped>
.list-item {
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 75vh;
  margin: auto;
  padding: 10px;
}
</style>