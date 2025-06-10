<template>
  <div class="habit-card">
    <label>
      <input type="checkbox" class="circle" v-model="habit.completed" @click="$emit('toggle')" />
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
    