<script setup>
import { defineProps, defineEmits, ref } from 'vue'

const props = defineProps(['editor', 'taskInput'])
const emits = defineEmits(['editTheTask'])

const taskInputRef = ref(props.taskInput)
let showEditor = ref(props.editor)

const changeInput = () => {
    showEditor.value = !showEditor.value
    emits('editTheTask', {value:taskInputRef, showEditor: showEditor })
}
</script>

<template>
    <div class="form-floating my-2 mx-3" v-if="showEditor">
        <input @keydown.enter.stop="changeInput" v-model="taskInputRef"  class="form-control form-control-lg" type="text" placeholder=".form-control-lg" aria-label=".form-control-lg example"/>
        <label for="floatingTextarea2">Task</label>
    </div>
</template>

<style>
textarea {
    resize: none;
}
</style>
