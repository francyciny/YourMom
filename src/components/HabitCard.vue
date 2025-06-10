<template>
    <div class="habit-card">
      <label>
        <input type="checkbox" v-model="habit.completed" @click="$emit('toggle')" />
        {{ habit.name }}
      </label>
      <span class="bell" @click="$emit('toggle-reminder')">
        {{ habit.reminder ? '🔔' : '🔕' }}
      </span>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      habit: Object
    }
  };
  </script>
  
  <style scoped>
  .habit-card input[type="checkbox"]{
  border-radius: 10%;
  width: 12px;
  height: 12px;
  accent-color: #007aff;
}
  .habit-card {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  .bell {
    cursor: pointer;
  }
  </style>
  