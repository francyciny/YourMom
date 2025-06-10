<template>
    <div class="container">
      <h1 class="title">Habits</h1>
      <div class="mom-quote-box">
        <p class="mom-quote">{{ momQuote }}</p>
      </div>
      <img :src="momImage" alt="Mom" class="mom-image" />
      <div class="calendar-bar" @click="calendarBack">
    <button class="calendar-arrow"><</button>
    <span class="calendar-date">Today</span>
    <button class="calendar-arrow">></button>
    </div>

    <div v-if="showCalendar" class="calendar">
    Calendar function has not been implemented yet
    </div>
      <div v-if="habits.length === 0 && showCalendar === false" class="empty-message">No habits yet</div>
      <br>
      <br>
      <HabitCard v-if="showCalendar === false"
  v-for="(habit, index) in habits"
  :key="index"
  :habit="habit"
  @toggle="toggleHabit(index)"
  @toggle-reminder="toggleReminder(index)"
/>
  
<div v-if="showCalendar === false" class="add-habit-wrapper">
  <div class="plus-icon-box">
    <button class="add-habit-btn" @click="showAddModal = true">+</button>
    </div>
  <button class="add-habit-btn" @click="showAddModal = true">Add new habit</button>
</div>
      
  
      <AddHabitModal
  v-if="showAddModal"
  :existingHabits="habits"
  @close="showAddModal = false"
  @add="addHabits"
/>
    </div>
  </template>
  
  <script>
  import HabitCard from './HabitCard.vue';
  import AddHabitModal from './AddHabitModal.vue';
  
  export default {
    components: {
      HabitCard,
      AddHabitModal,
    },
    data() {
      return {
        habits: [],
        showAddModal: false,
        showCalendar: false,
      };
    },
    computed: {
      momImage() {
        const completed = this.habits.filter((h) => h.completed).length;
        if (completed === this.habits.length && completed > 0) return '/mom-happy.jpg';
        if (completed/this.habits.length < 1/3 || this.habits.length === 0) return '/mom-angry.jpg';
        if (completed/this.habits.length < 2/3) return '/mom-sad.jpg';
        if (completed/this.habits.length < 1) return '/mom-neutral.jpg';
      },
      momQuote() {
        const completed = this.habits.filter((h) => h.completed).length;
        if (completed === this.habits.length && completed > 0) return "Good job...for today at least";
        if (completed/this.habits.length <1/3 || this.habits.length === 0) return "I'm so angry, get your stuff done now!";
        if (completed/this.habits.length <2/3) return "Slightly better, still you are not doing enough!";
        if (completed/this.habits.length < 1) return "Almost good, I am not satisfied yet!";
      },
    },
    methods: {
        toggleHabit(index) {
            console.log(this.habits[index]);
            this.habits[index].completed = !this.habits[index].completed;
            const completedCount = this.habits.filter(h => h.completed).length;

            if (completedCount === this.habits.length && this.habits.length > 0) {
                this.momMood = 'happy';
                this.momMessage = '“Good job...for today at least”';
            } else if (completedCount === 0) {
                this.momMood = 'angry';
                this.momMessage = '"I\'m so angry, get your stuff done now!"';
            } else {
                this.momMood = 'neutral';
                this.momMessage = '"You can do better!"';
            }
            },

      addHabits(newHabits) {
        this.habits = [...this.habits, ...newHabits];
        this.showAddModal = false;
      },
      toggleReminder(index) {
        this.habits[index].reminder = !this.habits[index].reminder;
        },
        calendarBack() {
            this.showCalendar = !this.showCalendar;
        },
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 390px;
    margin: 0 auto;
    padding: 1rem;
    text-align: center;
  }
  .title {
    font-size: 24px;
    margin-bottom: 10px;
    text-align: left;
  }
  .mom-image {
    width: 120px;
    margin: 5px auto;
  }
  .quote {
    font-style: italic;
  }
  .empty-message {
    margin-top: 2rem;
    color: gray;
  }
  .add-button {
    margin-top: 20px;
    padding: 10px 15px;
  }
  .calendar-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px auto;
  width: 100%;
  max-width: 360px;
  padding: 0.5rem;
  font-size: 18px;
  background-color: #f9f9f9;
  border-radius: 6px;
  border: 1px solid #ddd;
}

.calendar-arrow {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  padding: 0 10px;
}

.calendar-date {
  font-weight: bold;
}

.mom-quote-box {
  max-width: 260px;
  margin: 10px auto;
  background-color: #ffeef0;
  border: 2px solid #faa;
  border-radius: 20px;
  padding: 10px 15px;
  position: relative;
  font-style: italic;
}

.mom-quote-box::after {
  content: '';
  position: absolute;
  bottom: -15px;
  right: 30px;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-top-color: #ffeef0;
  border-bottom: 0;
  margin-left: -10px;
}

.add-habit-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  gap: 10px;
}

.plus-icon-box {
  width: 28px;
  height: 28px;
  background-color: rgb(239, 239, 239);
  color: #007aff;
  font-size: 20px;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.add-habit-btn {
  background: none;
  border: none;
  color: #007aff;
  font-size: 16px;
  cursor: pointer;
}
  </style>
  