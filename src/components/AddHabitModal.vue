<template>
  <div class="modal-overlay">
    <div class="modal-content">
      <div class="modal-header">
        <button @click="closeModal" class="ios-close"> <img class="icon" src="/system-icons/ios-close.png"> </button>
        <h2>New habits</h2>
        <button class="modal-add" @click="addHabits">Add</button>
      </div>
      <div class="modal-table">
        <div class="modal-row header">
          <p class="left-align">Select the new habits you want to track</p>
          <p class="right-align">Receive <br> reminders</p>
        </div>
        <ul class="habit-list">
          <li v-for="(habit, index) in filteredHabits" :key="index" class="habit-list-item">
            <label class="habit-label">
              <input type="checkbox" class="circle" v-model="habit.selected" />
                <span class="habit-name">{{ habit.name }}</span>
            </label>
            <label class="reminder-icon" @click="habit.reminder = !habit.reminder">
              {{ habit.reminder ? '🔔' : '🔕' }}
            </label>
          </li>
        </ul>
      </div>
      <p class="quote">Your mom is gonna be proud of you</p>
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
        document.getElementsByClassName("modal-content")[0].classList.add("slide-down");
        setTimeout(() => {
          this.$emit('close');
        }, 300);
      },
      addHabits() {
        const selectedHabits = this.availableHabits
          .filter((h) => h.selected)
          .map((h) => ({ name: h.name, completed: false, reminder: h.reminder }));
          document.getElementsByClassName("modal-content")[0].classList.add("slide-down");
        setTimeout(() => {
          this.$emit('add', selectedHabits);
        }, 300);
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
    align-items: flex-end;
    z-index: 100;
  }

  .modal-content {
    background: #fefefe;
    padding: 2rem;
    width: 100%;
    max-width: 325px;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    animation: slideUp 0.3s ease-out;
  }

  @keyframes slideUp {
    from {
      transform: translateY(100%);
    }
    to {
      transform: translateY(0);
    }
  }

  .modal-content.slide-down {
    animation: slideDown 0.3s ease-in;
  }

  @keyframes slideDown {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(100%);
    }
  }

  .modal-content h2 {
    font-size: 20px;
    font-weight: 600;
    text-align: center;
    margin-bottom: 1rem;
  }

  input[type="checkbox"].circle {
    appearance: none;
    -webkit-appearance: none;
    width: 22px;
    height: 22px;
    border: 2px solid lightgrey;
    border-radius: 100%;
    outline: none;
    cursor: pointer;
    position: relative;
    background-color: white;
    display: inline-block;
    vertical-align: middle;
  }

  input[type="checkbox"].circle:checked {
    background-color: #007aff;
    border: 2px solid #007aff;
  }

  input[type="checkbox"].circle:checked::after {
    content: '';
    position: absolute;
    top: 2px;
    left: 6px;
    width: 5px;
    height: 10px;
    border: solid white;
    border-width: 0 2px 2px 0;
    transform: rotate(40deg);
  }

  .habit-list {
    list-style: none;
    padding: 0;
    margin: 1rem 0;
    max-height: 250px;
    overflow-y: scroll;
    scrollbar-width: auto;
    scrollbar-color: #ccc transparent;
  }

  .habit-list::-webkit-scrollbar {
    width: 8px;
  }

  .habit-list::-webkit-scrollbar-thumb {
    background-color: #ccc;
    border-radius: 4px;
  }

  .habit-list-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 6px 0;
    border-bottom: 1px solid #eee;
  }

  .habit-label {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .habit-name {
    font-size: 16px;
  }

  .reminder-icon {
    cursor: pointer;
    font-size: 18px;
  }

  .modal-actions {
    margin-top: 1rem;
    display: flex;
    justify-content: space-between;
    gap: 10px;
  }

  .modal-actions button {
    flex: 1;
    background-color: #007aff;
    color: white;
    border: none;
    padding: 10px;
    border-radius: 10px;
    font-weight: 500;
    cursor: pointer;
    transition: background 0.2s ease;
  }

  .modal-actions button:first-child {
    background-color: #ddd;
    color: #333;
  }

  .ios-close {
    font-size: 20px;
    border-radius: 0px;
    border:none;
    background: transparent;
    color: #666;
    padding: 0.25rem;
  }

  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bold;
  }
  .modal-add {
    color: #007aff;
    background: none;
    border: none;
    font-size: 16px;
    cursor: pointer;
  }
  .modal-table {
    margin-top: 1rem;
  }
  .modal-row {
    display: flex;
  }
  .modal-row.header {
    font-size: 12px;
    font-weight: lighter;
    color: #888;
    border-bottom: none;
    text-transform: uppercase;
  }
  .left-align {
    flex: 1;
    text-align: left;
  }
  .right-align {
    flex: 1;
    text-align: right;
  }

  .quote {
    margin-top: 1rem;
    text-align: left;
    color: #888;
    font-size: 12px;
  }
  .icon {
    height: 30px;
  }
</style>
