<script setup>
import { ref, defineEmits } from 'vue';

const input = ref('');
const emit = defineEmits(['inputSubmit']);

const handleInput = () => {
    emit('inputSubmit', input.value);
    input.value=""
};
</script>

<template>
    <input type="text" class="form-control" v-model="input" @keydown.enter="handleInput">
</template>

<style scoped>
input{
    margin-top: 2em;
}
</style>
