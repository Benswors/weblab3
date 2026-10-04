<script setup lang="ts">
import { ref, computed } from 'vue'
import UserCard from './UserCard.vue'
import usersData from '../data/users.json'
import type { User } from '../types/User'

const users = ref<User[]>(usersData)
const genderFilter = ref('all')
const ageFilter = ref('all')
const sortType = ref('')

const filteredAndSorted = computed(() => {
  let res = [...users.value]

  if (genderFilter.value !== 'all') {
    res = res.filter(u => u.gender === genderFilter.value)
  }
  if (ageFilter.value === '18+') {
    res = res.filter(u => u.dob.age >= 18)
  }

  if (sortType.value === 'name-asc') res.sort((a, b) => a.name.first.localeCompare(b.name.first))
  if (sortType.value === 'name-desc') res.sort((a, b) => b.name.first.localeCompare(a.name.first))
  if (sortType.value === 'age-asc') res.sort((a, b) => a.dob.age - b.dob.age)
  if (sortType.value === 'age-desc') res.sort((a, b) => b.dob.age - a.dob.age)

  return res
})

const reset = () => {
  genderFilter.value = 'all'
  ageFilter.value = 'all'
  sortType.value = ''
}
</script>

<template>
  <div class="container">
    <!-- Тулбар -->
    <div class="toolbar">
      <div class="group">
        <span>Стать:</span>
        <button @click="genderFilter = 'all'" :class="{ active: genderFilter === 'all' }">Всі</button>
        <button @click="genderFilter = 'male'" :class="{ active: genderFilter === 'male' }">Чоловіки</button>
        <button @click="genderFilter = 'female'" :class="{ active: genderFilter === 'female' }">Жінки</button>
      </div>

      <div class="group">
        <span>Вік:</span>
        <button @click="ageFilter = 'all'" :class="{ active: ageFilter === 'all' }">Всі</button>
        <button @click="ageFilter = '18+'" :class="{ active: ageFilter === '18+' }">18+</button>
      </div>

      <div class="group">
        <span>Сортування:</span>
        <button @click="sortType = 'name-asc'" :class="{ active: sortType === 'name-asc' }">Ім'я ↑</button>
        <button @click="sortType = 'name-desc'" :class="{ active: sortType === 'name-desc' }">Ім'я ↓</button>
        <button @click="sortType = 'age-asc'" :class="{ active: sortType === 'age-asc' }">Вік ↑</button>
        <button @click="sortType = 'age-desc'" :class="{ active: sortType === 'age-desc' }">Вік ↓</button>
      </div>

      <button class="reset-btn" @click="reset">Очистити все</button>
    </div>

    
    <div v-if="filteredAndSorted.length === 0" class="empty">
      Список юзерів пустий
    </div>

    
    <div v-else class="grid">
      <UserCard v-for="user in filteredAndSorted" :key="user.id" :user="user" />
    </div>
  </div>
</template>

<style scoped>
.container { max-width: 1200px; margin: 0 auto; padding: 30px 20px; font-family: sans-serif; }
h2 { color: #1e293b; margin-bottom: 20px; }

.toolbar { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 16px; margin-bottom: 24px; display: flex; flex-wrap: wrap; gap: 16px; align-items: center; box-shadow: 0 2px 4px rgba(0,0,0,0.02); }
.group { display: flex; align-items: center; gap: 6px; font-size: 0.85rem; color: #64748b; font-weight: bold; }
.group span { margin-right: 4px; }

button { background: #f8fafc; border: 1px solid #cbd5e1; padding: 6px 12px; border-radius: 6px; font-size: 0.8rem; cursor: pointer; color: #334155; font-weight: 500; transition: 0.2s; }
button:hover { background: #f1f5f9; }
button.active { background: #2563eb; color: white; border-color: #2563eb; }

.reset-btn { background: #fef2f2; color: #dc2626; border-color: #fca5a5; margin-left: auto; }
.reset-btn:hover { background: #fee2e2; }

.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; }
.empty { text-align: center; padding: 40px; color: #64748b; font-size: 1.1rem; background: #fff; border-radius: 12px; border: 1px solid #e2e8f0; }
</style>