<template>
  <div class="container">
    <h1 class="title">Habits</h1>
    <div class="mom-quote-box">
      <p class="mom-quote">{{ momQuote }}</p>
    </div>
    <img :src="momImage" alt="Mom" class="mom-image" />
    <div class="date-nav">
      <button class="nav-btn left" @click="calendarBack()">&lt;</button>
      <span v-if="!showCalendar" class="today-label">Today</span>
      <button class="nav-btn right" @click="calendarBack()">&gt;</button>
    </div>
    <div v-if="showAlert" class="alert-overlay">
      <div class="alert-content">
        <p class="alert-title">You have added the maximum number of habits (6) available in this version.</p>
        <div class="alert-actions">
            <button @click="noAlert()" class="alert-button got-it">Got it!</button>
        </div>
      </div>
    </div>
    <div v-if="showCalendar" class="calendar">Calendar function has not been implemented yet</div>
    <div v-if="habits.length === 0 && showCalendar === false" class="empty-message">No habits yet</div>
    <br>
    <br>
    <div class="habits-scrollable">
      <HabitCard v-if="showCalendar === false"
        v-for="(habit, index) in habits"
        :key="index"
        :habit="habit"
        @toggle="emitToggleHabit(index)"
        @toggle-reminder="emitToggleReminder(index)"
      />
    </div>
    <div v-if="showCalendar === false" class="add-habit-wrapper">
      <div class="plus-icon-box">
        <button class="add-habit-btn" @click="showAdd()">+</button>
      </div>
      <button class="add-habit-text" @click="showAdd()">Add new habit</button>
    </div>
    <AddHabitModal
      v-if="showAddModal"
      :existingHabits="habits"
      @close="showAddModal = false"
      @add="emitAddHabits"
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
    props: {
      habits: {
        type: Array,
        default: () => [],
      },
    },
    emits: ['update:habits'], 
    data() {
      return {
        showAddModal: false,
        showCalendar: false,
        showAlert: false,
      };
    },
    computed: {
      momImage() {
        const completed = this.habits.filter((h) => h.completed).length;
        if (completed === this.habits.length && completed > 0) return '/habits/mom-happy.jpg';
        if (this.habits.length === 0) return '/habits/mom-angry.jpg'; // If no habits, mom is angry
        if (completed / this.habits.length < 1 / 3) return '/habits/mom-angry.jpg';
        if (completed / this.habits.length < 2 / 3) return '/habits/mom-sad.jpg';
        if (completed / this.habits.length < 1) return '/habits/mom-neutral.jpg';
        return '/habits/mom-angry.jpg'; // Fallback
      },
      momQuote() {
        const completed = this.habits.filter((h) => h.completed).length;
        if (this.habits.length === 0) return "You haven't added any habits yet...what are you waiting for!";
        if (completed === this.habits.length) return "Good job...for today at least...better do it again tomorrow!";
        if (completed / this.habits.length < 1 / 3) return "I'm so angry, get your stuff done now!";
        if (completed / this.habits.length < 2 / 3) return "Slightly better, still you are not doing enough!";
        if (completed / this.habits.length < 1) return "Almost good, I am not satisfied yet, I'm sure you can do it all!";
        return "You haven't added any habits yet...what are you waiting for!"; 
      },
    },
    methods: {
      emitToggleHabit(index) {
        const updatedHabits = [...this.habits];
        updatedHabits[index].completed = !updatedHabits[index].completed;
        this.$emit('update:habits', updatedHabits); 
      },
      emitAddHabits(newHabits) {
        const updatedHabits = [...this.habits, ...newHabits];
        this.showAddModal = false;
        this.$emit('update:habits', updatedHabits); 
      },
      showAdd() {
        if (this.habits.length < 6) {
          this.showAddModal = true;
        } else {
          this.showAlert = true;
        }
      },
      noAlert() {
        this.showAlert = false;
      },
      emitToggleReminder(index) {
        const updatedHabits = [...this.habits];
        updatedHabits[index].reminder = !updatedHabits[index].reminder;
        this.$emit('update:habits', updatedHabits);
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
    font-size: 28px;
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
    margin-top: 10px;
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
    color: #007aff ;
    font-size: 16px;
    cursor: pointer;
  }

  .add-habit-text {
    background: none;
    border: none;
    color: black ;
    font-size: 16px;
    cursor: pointer;
  }

  .date-nav {
    position: relative;
    height: 40px;
    margin: 5px 0 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .today-label {
    font-size: 18px;
    color: #333;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }

  .nav-btn {
    background: none;
    border: none;
    font-size: 24px;
    color: #007aff;
    cursor: pointer;
    position: absolute;
    top: 0;
  }

  .nav-btn.left {
    left: 0px;
  }

  .nav-btn.right {
    right: 0px;
  }

  .habits-scrollable {
    max-height: 160px; 
    overflow-y: auto;
    margin-top: 10px;
    padding-right: 5px;
  }

  .alert-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 100;
    padding: 1rem; 
  }

  .alert-content {
    background: #fefefe;
    padding: 1rem; 
    width: 100%;
    max-width: 300px; 
    border-radius: 20px; 
    box-shadow: 0 4px 20px rgba(0,0,0,0.1); 
    text-align: center;
  }

  .alert-title {
    font-size: 16px;
    font-weight: 500;
    text-align: center;
    margin-bottom: 1rem;
    color: #000;
  }

  .alert-actions {
    display: flex;
    flex-direction: row; 
    border-top: 1px solid #eee; 
  }

  .alert-button {
    background: none;
    border: none;
    padding: 0.25rem;
    font-size: 1.05rem;
    cursor: pointer;
    width: 100%; 
    text-align: center;
  }

  .alert-button.got-it {
    color: #007AFF; 
    font-weight: normal; 
  }
</style>