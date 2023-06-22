<script setup>
import { ref, onMounted, watch } from "vue"
import InputVue from "./components/input.vue"
import TaskHolder from "./components/taskHolder.vue"
import Editor from "./components/Editor.vue"
// localStorage.setItem("taskData", JSON.stringify([{name: "playing game", id: Date.now(), completed:false}, { name: "playing game", id: Date.now()+4354, completed: false }, { name: "playing game", id: Date.now()+566, completed: false }]))

let allTasks = ref([]);
const showEdit = ref(false)
const EditTaskInput = ref("")
let taskId = ref("")

if (localStorage.getItem('taskData')) {
  allTasks.value = JSON.parse(localStorage.getItem("taskData"))
}

// console.log(allTasks)
const handleInputSubmit = (value) => {
  if (value.length > 0) {
    const taskObj = {
      name: value,
      id: Date.now(),
      completed: false
    }

    allTasks.value.push(taskObj);
    localStorage.setItem("taskData",JSON.stringify(allTasks.value))
  } else {
    alert("Please enter a task")
  }
}

watch(allTasks, (val) => {
  localStorage.setItem("taskData", JSON.stringify(val))
})
watch(EditTaskInput, (task) => {
  
})

const deleteTask = ({ id, index }) => {
  // localStorage.setItem("taskData", JSON.stringify(allTasks.value.splice(index, 1)))
  // console.log(allTasks.value.splice(index, 1))
  allTasks.value.splice(index, 1)
  localStorage.setItem('taskData',JSON.stringify(allTasks.value));
}
const changeStateTask = ({ id, index }) => {
  allTasks.value.map((taskItem) => {
    if (taskItem.id === id) {
      const newCompletedValue = !taskItem.completed;
      taskItem.completed = newCompletedValue;
     }
  })
  return localStorage.setItem("taskData", JSON.stringify(allTasks.value))
  
}
const handleChangeTask = ({ id, index, showEditor }) => {
    // console.log(showEditor)
    allTasks.value.map((item) => {
      if (item.id == id) {
      EditTaskInput.value = item.name
      taskId = item.id
      
      }
    showEdit.value = showEditor.value
  })
  return localStorage.setItem("taskData", JSON.stringify(allTasks.value))
}
const changeTaskValue = ({ value, showEditor }) => {

  EditTaskInput.value = value
  allTasks.value.filter((item) => {
    if (item.id == taskId) {
      item.name = EditTaskInput.value
    }
  })
  showEdit.value = showEditor
  return localStorage.setItem("taskData", JSON.stringify(allTasks.value))
}
</script>

<template>
  <div>
    <Editor :editor="showEdit" :taskInput="EditTaskInput" @editTheTask="changeTaskValue" v-if="showEdit"></Editor>
    <TaskHolder :tasks="allTasks" :editor="showEdit" @delete="deleteTask" @changeState="changeStateTask" @changeTask="handleChangeTask" ></TaskHolder>
    <InputVue @inputSubmit="handleInputSubmit"></InputVue>
  </div>
</template>


