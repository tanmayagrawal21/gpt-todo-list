<template>
  <v-container> 
  <h1 class="text-center"> To Do List </h1>
    <!-- each task encapsulated in a box with option to delete -->
    <v-container v-for="(task, index) in tasks" :key="index">
      <!--delete button-->
      <v-btn icon @click="tasks.splice(index, 1)">
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

    ]
  }),
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