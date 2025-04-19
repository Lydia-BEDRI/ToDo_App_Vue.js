<script setup>
import { ref , reactive, computed} from 'vue';
import Task from './components/Task.vue';
import Filter from './components/Filter.vue';
import Modalwindow from './components/Modalwindow.vue';
const appName = "To-Do App";
const tasks= reactive([
    {
      id: 1,
      name: "Website design",
      description: "Define the style guide, branding and create the webdesign on Figma.",
      completed: true
    },
    {
      id: 2,
      name: "Website development",
      description: "Develop the portfolio website using Vue JS.",
      completed: false
    },
    {
      id: 3,
      name: "Hosting and infrastructure",
      description: "Define hosting, domain and infrastructure for the portfolio website.",
      completed: false
    },
    {
      id: 4,
      name: "Composition API",
      description: "Learn how to use the composition API and how it compares to the options API.",
      completed: true
    },
    {
      id: 5,
      name: "Pinia",
      description: "Learn how to setup a store using Pinia.",
      completed: true
    },
    {
      id: 6,
      name: "Groceries",
      description: "Buy rice, apples and potatos.",
      completed: false
    },
    {
      id: 7,
      name: "Bank account",
      description: "Open a bank account for my freelance business.",
      completed: false
    }
]);

let newTask = reactive({
  completed: false
});

function addTask() {
  if (newTask.name && newTask.description) {
    newTask.id = Math.max(...tasks.map(task => task.id)) + 1;
    tasks.push(newTask);

    newTask = {completed: false};
  }
  else {
    alert("Please fill in all fields");
  }
}

function toggleCompleted(id) {
  const task = tasks.find(task => task.id === id);
  if (task) {
    task.completed = !task.completed;
  }
}

const filterBy = ref("");

function setFilter(value) {
  filterBy.value = value;
} 

const filteredTasks = computed(() => {
  switch (filterBy.value) {
    case "todo":
      return tasks.filter(task => !task.completed);
    case "done":
      return tasks.filter(task => task.completed);
    default:
      return tasks;
  }
});
let modalIsActive = ref(false);
</script>

<template>

  <main class="container">
    <div class="header">
      <div class="header-side">
        <h1>
          {{ appName }}
        </h1>
      </div>
      <div class="header-side">
        <button @click="modalIsActive = true" class="btn secondary">+ Add Task</button>
    </div>
    </div>
    
      <Filter  :filterBy = "filterBy" @setFilter="setFilter"/>
   

    <div class="tasks">
      
      <Task  @toggleCompleted="toggleCompleted" v-for="(task, index) in filteredTasks" :key="index" :task="task" />
    </div>

    <div class="add-task">
      <h3>Add a new task</h3>
      <input v-model="newTask.name" type="text" name="title" placeholder="Enter a title..."><br />
      <textarea v-model="newTask.description" name="description" rows="4" placeholder="Enter a description..." /><br />
      <button @click="addTask" class="btn gray">Add Task</button>

    </div>
    <Modalwindow v-if="modalIsActive" @closePopup="modalIsActive=false"/>
  </main>
  
   

</template>


<style lang="scss" scoped>

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  .header-side {
    display: flex;
    align-items: center;

    h1 {
      text-transform: capitalize;
      font-size: 42px;
      font-weight: 700;
      line-height: 47px;
      letter-spacing: 0em;
      text-align: left;
    }

    .secondary {
      margin-left: 12px;
    }
  }

}


.tasks {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;

  @media (max-width: 768px) {
    grid-template-columns: repeat(1, 1fr);
  }
}


</style>