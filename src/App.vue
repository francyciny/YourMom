<template>
  <div style="max-width: 390px; height: 100vh; margin: auto; display: flex; flex-direction: column; font-family: sans-serif; border: 1px solid #ccc;">
    <!-- Header -->
    <header class="phone-header">
    <div class="status-bar">
      <span class="time">9:41</span>
      <div class="status-icons">
        <img src="/icons.jpg" alt="Cell" class="icon" />
      </div>
    </div>
    <div class="notch"></div>
    <!--<h1 class="app-title">YourMom</h1>-->
  </header>

    <!-- Tab Content -->
    <main style="flex-grow: 1; overflow-y: hidden; padding: 1rem;">
      <HabitsPage v-if="currentTab === 'habits'" />
      <ChallengesPage v-if="currentTab === 'challenges'" />
      <RecipesPage v-if="currentTab === 'recipes'" />
    </main>

    <!-- Bottom Nav -->
    <nav style="display: flex; border-top: 1px solid #ccc; ">
      <button
        v-for="tab in tabs"
        :key="tab.name"
        @click="currentTab = tab.name"
        :style="tabButtonStyle(tab.name)"
      >
      <img :src="currentTab === tab.name ? tab.activeImage : tab.image" style="width: 20px;" />
      <br />
      <span :style="{ color: currentTab === tab.name ? '#007aff' : '#808080' }">
        {{ tab.label }}
      </span> 
      </button>
    </nav>
    <div class="home-indicator"></div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import HabitsPage from './components/HabitsPage.vue'
import ChallengesPage from './components/ChallengesPage.vue'
import RecipesPage from './components/RecipesPage.vue'

const currentTab = ref('habits')

const tabs = [
  {
    name: 'habits',
    label: 'Habits',
    image: '/habits-page.png',
    activeImage: '/habits-page-active.png',
  },
  {
    name: 'challenges',
    label: 'Challenges',
    image: '/challenges-page.png',
    activeImage: '/challenges-page-active.png',
  },
  {
    name: 'recipes',
    label: 'Recipes',
    image: '/recipes-page.png',
    activeImage: '/recipes-page-active.png',
  },
]

const currentTabTitle = computed(() => {
  const tab = tabs.find(t => t.name === currentTab.value)
  return tab ? tab.label : ''
})

const tabButtonStyle = (tabName) => ({
  flex: 1,
  padding: '0.75rem',
  fontWeight: currentTab.value === tabName ? 'bold' : 'normal',
  background: currentTab.value === tabName ? '#fff' : '#fff',
  border: 'none',
})
</script>

<style scoped>
input[type="checkbox"] {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  accent-color: #007aff; 
}

.phone-header {
  position: relative;
  background-color: white;
  padding-top: 1.2rem;
  padding-bottom: 0.5rem;
  text-align: center;
  border-bottom: 1px solid #ccc;
}

.status-bar {
  display: flex;
  justify-content: space-between;
  padding: 0 1rem;
  font-size: 12px;
  color: black;
  margin-bottom: 0.3rem;
}

.status-icons {
  display: flex;
  gap: 6px;
  align-items: center;
}

.icon {
  height: 20px;
}
.time {
  font-weight: bold;
  font-size: 14px;
}
.notch {
  position: absolute;
  top: 0.8rem;
  left: 50%;
  transform: translateX(-50%);
  width: 120px;
  height: 20px;
  background: black;
  border-radius: 10px;
  z-index: 1;
}

.app-title {
  font-size: 1.4rem;
  font-weight: bold;
  position: relative;
  z-index: 2;
}
.home-indicator {
  width: 135px;
  height: 5px;
  background-color: black;
  border-radius: 3px;
  margin: 6px auto 10px auto;
}

</style>