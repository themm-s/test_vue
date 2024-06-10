<script setup lang="ts">
import { ref } from 'vue'

const name = ref('')
const status = ref('')

</script>

<template>
    <div class="filter-item">
        <input type="text" placeholder="Name" v-model="name" />
        <input type="text" placeholder="Status" v-model="status" />
        <button @click="$emit('applyFilter', {name, status})">Применить</button>
    </div>
</template>

<style>
.filter-item {
    display: flex;
    gap: 12px;
}

</style>