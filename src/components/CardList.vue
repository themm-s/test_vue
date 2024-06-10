<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { TResults } from '../types/results';
import FilterItem from './FilterItem.vue';
import Pagination from './Pagination.vue';
import StatusIcon from './StatusIcon.vue';

const list = ref<TResults>()
const currentPage = ref(1)
const totalPages = ref()
const loading = ref(true)

onMounted(async () => {
  await fetch('https://rickandmortyapi.com/api/character', {
    method: 'GET',
  })
    .then(res => res.json())
    .then(data => { list.value = data.results; totalPages.value = data.info.pages; loading.value = false; })
    .catch(err => console.log(err))
  await console.log(list.value)
})

const applyFilter = async (filter: { name: string, status: string }) => {
  loading.value = true
  await fetch(`https://rickandmortyapi.com/api/character?name=${filter.name}&status=${filter.status}`, {
    method: 'GET',
  }).then(res => res.json())
    .then(data => { list.value = data.results; loading.value = false; })
    .catch(err => console.log(err))
}

const changePage = async (page: number) => {
  currentPage.value = page
  loading.value = true
  await fetch(`https://rickandmortyapi.com/api/character?page=${page}`, {
    method: 'GET',
  }).then(res => res.json())
    .then(data => { list.value = data.results; loading.value = false; })
}

</script>

<template>
  <div v-if="loading">Loading...</div>
  <div style="color: red; margin: auto; text-align: center; font-size: large; font-weight: 900" v-else-if="list === undefined && !loading">Error</div>
  <div v-else>
  <FilterItem @applyFilter="applyFilter($event)" />
  <section class="list">
    <div class="card" v-for="item in list">
      <div class="card-img">
        <img :src="item.image">
      </div>
      <div class="card-info">
        <div class="card-header section">
          <h1>{{ item.name }}</h1>
          <span class="status">
            <StatusIcon :status="item.status" />
            {{ item.status }}- {{ item.species }}
          </span>
        </div>
        <div class="card-body section">
          <span>Last known location:</span>
          <p class="card-location">{{ item.location.name }}</p>
        </div>
        <div class="card-footer section">
          <span>First seen in:</span>
          <p class="card-location">{{ item.origin.name }}</p>
        </div>
      </div>
    </div>
  </section>
</div>
  <Pagination :currentPage="currentPage" :totalPages="totalPages" @changePage="changePage($event)" />
</template>

<style>
span {
  color: rgba(0, 0, 0, 0.6)
}

.list {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  width: 100%;
  gap: 20px;
  justify-content: center;
}

.card {
  display: flex;
  width: 500px;
  height: 250px;
  padding: 0;
  overflow: hidden;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
  border-radius: 12px;
}

.card-img {
  flex: 2 1 0%;
  width: 100%;
}

.card-info {
  display: flex;
  padding: 1rem;
  flex-direction: column;
  flex: 3 1 0%;
  width: 100%;
}

.card-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center center;
}

.list-item {
  height: 80px;
  word-wrap: initial;
  font-size: 20px;
}

.status {
  display: flex;
  flex-direction: row;
  align-items: center;
  font-size: 16px;
  font-weight: 700;
}

.card-header {
  display: flex;
  flex-direction: column;
}

.card-body {
  display: flex;
  flex-direction: column;
}

.card-location {
  font-size: 16px;
  color: black;
  font-weight: 500;
}
</style>
