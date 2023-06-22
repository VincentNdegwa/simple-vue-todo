<script setup>
import { ref,defineProps, computed, defineEmits } from "vue"
const props = defineProps(['tasks','editor'])
const tasks = ref(props.tasks)
let showEditor = ref(props.editor)
// computed
const completedTasks = computed(() => {
    return tasks.value?.filter((item) => item.completed == true )
})

const pendingTasks = computed(() => {
    return tasks.value?.filter((item) => item.completed == false)
})
// emmits
const emit = defineEmits(['delete','changeState','changeTask'])
const showId = (id,index) => {
  emit("delete", { id: id, index: index })
}
const getTicked = (id,index) => {
  emit("changeState", { id: id, index: index })
}
const getAdded = (id, index) => {
  showEditor.value = !showEditor.value
  emit("changeTask", { id: id, index: index, showEditor: showEditor })
}

</script>
<template>
   <ol class="list-group list-group-numbered mt-5 mx-5">
      <h3 v-if="completedTasks.length">Completed Tasks</h3>
      <li v-if="completedTasks.length" v-for="(task, index) in completedTasks" :key="task.id+'-'+index" class="list-group-item d-flex justify-content-between align-items-start">
        <div class="ms-2 me-auto">
          <div class="fw-bold">{{ task.completed? 'Done': 'Pending' }} </div>
          {{ task.name }}
        </div>
        <div class="operations">
            <button @click="showId(task.id, index)">Delete</button>
            <button @click="getAdded(task.id, index)">Edit</button>
            <input @click="getTicked(task.id, index)" type="checkbox" v-model="task.completed" class="mt-5 mx-5"/>
        </div>
        
      </li>
    </ol>


      <ol class="list-group list-group-numbered mt-5 mx-5">
          <h3 v-if="pendingTasks.length" >Pending Tasks</h3>
          <li  v-if="pendingTasks.length" v-for="(task, index) in pendingTasks" :key="task.id+'-'+index" class="list-group-item d-flex justify-content-between align-items-center">
            <div class="ms-2 me-auto">
              <div class="fw-bold">{{ task.completed ? 'Done' : 'Pending' }} </div>
              {{ task.name }}
            </div>
             <div class="operations">
               <button @click="showId(task.id, index)">Delete</button>
               <button @click="getAdded(task.id, index)">Edit</button>
               <input @click="getTicked(task.id, index)" type="checkbox" v-model="task.completed" class="mt-5 mx-5"/>
            </div>
          </li>
        </ol>
</template>

<style scoped>
.operations>button{
    padding: 0.5em;
    border-radius: 0.4em;
    background: red;
    color: white;
}
</style>