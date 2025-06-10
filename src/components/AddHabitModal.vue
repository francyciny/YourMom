<template>
    <div class="modal-overlay">
      <div class="modal-content">
        <h2>Select Habits</h2>
        <ul class="habit-list">
            <li v-for="(habit, index) in filteredHabits" :key="index">
                <label>
                <input type="checkbox" v-model="habit.selected" />
                {{ habit.name }}
                </label>
                <label @click="habit.reminder = !habit.reminder" style="cursor: pointer;">
                {{ habit.reminder ? '🔔' : '🔕' }}
                </label>
            </li>
            </ul>
        <div class="modal-actions">
          <button @click="closeModal">Cancel</button>
          <button @click="addHabits">Add</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        availableHabits: [
          { name: 'Study', selected: false, reminder: false },
          { name: 'Shower', selected: false, reminder: false },
          { name: 'Work out', selected: false, reminder: false },
          { name: 'Drink water', selected: false, reminder: false },
          { name: 'Sleep early', selected: false, reminder: false },
          { name: 'Read a book', selected: false, reminder: false },
        ],
      };
    },
    props: {
        existingHabits: {
            type: Array,
            required: true
        }
        },
        computed: {
        filteredHabits() {
            return this.availableHabits.filter(
            (habit) => !this.existingHabits.some((h) => h.name === habit.name)
            );
        }
        },
    methods: {
      closeModal() {
        this.$emit('close');
      },
      addHabits() {
        const selectedHabits = this.availableHabits
          .filter((h) => h.selected)
          .map((h) => ({ name: h.name, completed: false, reminder: h.reminder }));
        this.$emit('add', selectedHabits);
      },
    },
  };
  </script>
  
  <style scoped>
  .modal-overlay {
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
  }
  
  .modal-content {
    background: white;
    padding: 1.5rem;
    width: 90%;
    max-width: 360px;
    border-radius: 8px;
  }

  .habit-list input[type="checkbox"] {
  border-radius: 0%;
  width: 14px;
  height: 14px;
  accent-color: #007aff;
    }
  
  .habit-list {
    list-style: none;
    padding: 0;
    margin: 1rem 0;
  }
  
  .habit-list li {
    display: flex;
    justify-content: space-between;
    margin-bottom: 0.5rem;
  }
  
  .modal-actions {
    display: flex;
    justify-content: space-between;
  }
  </style>
  