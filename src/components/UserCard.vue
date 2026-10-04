<script setup lang="ts">
import { ref, computed } from 'vue'
import type { User } from '../types/User'

const props = defineProps<{ user: User }>()
const show = ref(false)


const ageClass = computed(() => {
  const a = props.user.dob.age
  return {
    minor: a < 18,
    young: a >= 18 && a <= 30,
    adult: a >= 31 && a <= 50,
    senior: a > 50
  }
})
</script>

<template>
  <div class="card" :class="ageClass">
    <div class="card-top">
      <img :src="user.picture" :alt="`${user.name.first} ${user.name.last}`" class="avatar" />
      <div>
        <h3>{{ user.name.first }} {{ user.name.last }}</h3>
        <span class="city">{{ user.location.city }}, {{ user.location.country }}</span>
      </div>
    </div>

    <div class="info">
      <p>✉️ {{ user.email }}</p>
      <p>📞 {{ user.phone }}</p>
      <p v-if="user.dob.age > 18">🎂 Вік: {{ user.dob.age }}</p>
    </div>

    <div class="hobbies">
      <span v-for="(h, i) in user.hobbies" :key="i" class="tag">{{ h }}</span>
    </div>

    <button class="details-btn" @click="show = !show">
      {{ show ? 'Сховати деталі ▲' : 'Деталі ▼' }}
    </button>
    
    <div v-show="show" class="details-box">
      {{ user.details }}
    </div>
  </div>
</template>

<style scoped>
.card {
  background: var(--card-bg, #ffffff);
  border: 1px solid var(--border, #e2e8f0);
  border-radius: 12px;
  padding: 16px;
  width: 280px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s;
}
.card:hover { transform: translateY(-3px); }

.card-top { display: flex; align-items: center; gap: 12px; margin-bottom: 12px; }
.avatar { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; }
.card-top h3 { margin: 0; font-size: 1rem; color: var(--text, #1e293b); }
.city { font-size: 0.8rem; color: #64748b; }

.info { font-size: 0.85rem; color: #475569; display: flex; flex-direction: column; gap: 4px; margin-bottom: 10px; }
.info p { margin: 0; }

.hobbies { display: flex; flex-wrap: wrap; gap: 4px; margin-bottom: 12px; }
.tag { background: #f1f5f9; color: #334155; font-size: 0.75rem; padding: 2px 8px; border-radius: 4px; }

.details-btn { width: 100%; background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px; padding: 6px; font-size: 0.8rem; cursor: pointer; color: #2563eb; font-weight: 600; }
.details-btn:hover { background: #f1f5f9; }

.details-box { margin-top: 8px; font-size: 0.8rem; color: #64748b; background: #f8fafc; padding: 8px; border-radius: 6px; }


.minor { border-left: 4px solid #3b82f6; }
.young { border-left: 4px solid #10b981; }
.adult { border-left: 4px solid #f59e0b; }
.senior { border-left: 4px solid #ef4444; }
</style>