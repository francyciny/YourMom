<template>
  <div style="max-width: 390px; height: 100vh; margin: auto; display: flex; flex-direction: column; font-family: sans-serif; border: 1px solid #ccc;">
    <header class="phone-header">
      <div class="status-bar">
        <span class="time">9:41</span>
        <div class="status-icons">
          <img src="/system-icons/top-bar-icons.jpg" alt="Cell" class="icon" />
        </div>
      </div>
      <div class="notch"></div>
    </header>
    <main style="flex-grow: 1; overflow-y: hidden; padding: 1rem;">
        <HabitsPage v-if="currentTab === 'habits'" v-model:habits="habits"/>
        <ChallengesPage v-if="currentTab === 'challenges'" v-model:challenges="challenges" />
        <RecipesPage
          v-if="currentTab === 'recipes'"
          ref="recipesPageRef" @change-tab="currentTab = $event"
        />
    </main>
    <nav style="display: flex; border-top: 1px solid #ccc; ">
      <button
        v-for="tab in tabs"
        :key="tab.name"
        @click="handleTabClick(tab.name)" :style="tabButtonStyle(tab.name)"
      >
        <img :src="currentTab === tab.name ? tab.activeImage : tab.image" style="width: 28px;" />
        <br/>
        <span :style="{ color: currentTab === tab.name ? '#007aff' : '#808080' }">
          {{ tab.label }}
        </span> 
      </button>
    </nav>
    <div class="home-indicator"></div>
  </div>
</template>

<script>
  import HabitsPage from './components/HabitsPage.vue';
  import ChallengesPage from './components/ChallengesPage.vue';
  import RecipesPage from './components/RecipesPage.vue';

  export default {
    components: {
      HabitsPage,
      ChallengesPage,
      RecipesPage,
    },
    data() {
      return {
        currentTab: 'habits',
        tabs: [
          {
            name: 'habits',
            label: 'Habits',
            image: '/habits/habits-page.png',
            activeImage: '/habits/habits-page-active.png',
          },
          {
            name: 'challenges',
            label: 'Challenges',
            image: '/challenges/challenges-page.png',
            activeImage: '/challenges/challenges-page-active.png',
          },
          {
            name: 'recipes',
            label: 'Recipes',
            image: '/recipes/recipes-page.png',
            activeImage: '/recipes/recipes-page-active.png',
          },
        ],
        challenges: [], 
        habits: [],     
      };
    },
    created() {
      const storedHabits = sessionStorage.getItem('habits');
      if (storedHabits) {
        try {
          this.habits = JSON.parse(storedHabits);
        } catch (e) {
          console.error("Error parsing habits from sessionStorage:", e);
          this.habits = [];
        }
      } else {
        this.habits = []; 
      }

      const storedChallenges = sessionStorage.getItem('challenges');
      if (storedChallenges) {
        try {
          this.challenges = JSON.parse(storedChallenges);
        } catch (e) {
          console.error("Error parsing challenges from sessionStorage:", e);
          this.challenges = [
            { name: 'Eat at least one apple', completed: false },
            { name: 'Do 10 pushups', completed: false },
            { name: 'Cook one meal from the recipes', completed: false },
          ];
        }
      } else {
        this.challenges = [
          { name: 'Eat at least one apple', completed: false },
          { name: 'Do 10 pushups', completed: false },
          { name: 'Cook one meal from the recipes', completed: false },
        ];
      }
    },
    watch: {
      habits: {
        handler(newHabits) {
          sessionStorage.setItem('habits', JSON.stringify(newHabits));
        },
        deep: true, // Watch for changes inside the array (ex. a habit's completed status)
      },
      challenges: {
        handler(newChallenges) {
          sessionStorage.setItem('challenges', JSON.stringify(newChallenges));
        },
        deep: true, 
      },
    },
    methods: {
      tabButtonStyle(tabName) {
        if (this.currentTab === tabName) { 
          return {
            flex: 1,
            padding: '0.75rem',
            fontWeight: 'bold',
            background: '#fff',
            border: 'none',
          };
        } else {
          return {
            flex: 1,
            padding: '0.75rem',
            fontWeight: 'normal',
            background: '#fff',
            border: 'none',
          };
        }
      },
      handleTabClick(tabName) {
        if (this.currentTab === 'recipes' && tabName === 'recipes') {
          // Use $nextTick to ensure the component is mounted before trying to access its ref
          this.$nextTick(() => {
            if (this.$refs.recipesPageRef && typeof this.$refs.recipesPageRef.resetToMainView === 'function') {
              this.$refs.recipesPageRef.resetToMainView();
            }
          });
        }
        this.currentTab = tabName; 
      },
    },
  };
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