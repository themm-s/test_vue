<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps<{
    currentPage: number
    totalPages: number
}>()

const emit = defineEmits(['changePage'])

console.log(props.totalPages)

</script>

<template>
    <div class="pagination" v-if="totalPages !== undefined">
        <button class="pagination-item":class="{ 'active': i === currentPage }" v-for="i in totalPages" :key="i"
            @click="$emit('changePage', i)">
            {{ i }}
        </button>
    </div>
</template>

<style scoped>
.pagination-item {
    padding: 10px;
    border: 1px solid black;
    cursor: pointer;
}

.active {
    font-weight: bold;
    color: red;
}

.pagination {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 20px;
    margin: 0 auto;
    padding: 20px;
    justify-content: flex-start;
    align-items: center;
}
</style>