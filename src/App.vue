<template>
  <div id="app">

    <h1> To Do List </h1>
    <!-- each task encapsulated in a box with option to delete -->
    <ListItem v-for="(task, index) in tasks" :key="index" :task="task.task" :subtasks="task.subtasks" class="list-item"/>

    <!-- Add new list item-->
    <div class="list-item">
      <div class="list-item__title">
        <input type="text" placeholder="Add new task" />
      </div>
      <div class="list-item__subtitle">
        <input type="text" placeholder="Add new subtask" />
      </div>
    </div>

  </div>
</template>

<script>
import ListItem from './components/ListItem.vue'

export default {
  name: 'App',
  components: {
    ListItem
  }, 
  data() {
    return {
      tasks: [
        {
          task: 'Task 1',
          subtasks: [
            'Subtask 1',
            'Subtask 2',
            'Subtask 3']
        },
        {
          task: 'Task 2',
          subtasks: [
            'Subtask 1',
            'Subtask 2',
            'Subtask 3']
        },
        {
          task: 'Task 3',
          subtasks: [
          ]
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0 auto;
}

.list-item {
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 2vh;
  margin-left: auto;
  display: inline-block;
  width: 100%;
}

.list-item__title {
  font-size: 1.5em;
  font-weight: bold;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  text-align: left;
  padding-left: 5vw;
}

.list-item__subtitle {
  display: inline-block;
  width: 100%;
  margin-left: 0;
  padding-left: 0;
}

</style>
