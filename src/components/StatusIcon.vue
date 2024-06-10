<script setup lang="ts">
import { defineProps, ref } from 'vue';
const props = defineProps<{ status: string }>()
const color = ref()


if (!['Alive', 'Dead', 'unknown'].includes(props.status)) {
    console.error('Invalid status')
}

if (props.status === 'Alive') {
    color.value = 'alive'
} else if (props.status === 'Dead') {
    color.value = 'dead'
} else {
    color.value = 'unknown'
}

console.log(props.status)
</script>

<template>
    <div class="status-icon" :class="color"></div>
</template>

<style scoped>

.status-icon {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    margin-right: 5px;
}

.alive {
    background-color: green !important;
}

.dead {
    background-color: red !important;
}

.unknown {
    background-color: gray;
}
</style>