<template>
    <div class="container">
      <h1 class="title">Challenges</h1>
      <p class="text">I AM YOUR PET, COMPLETE DAILY CHALLENGES TO MAKE ME HAPPY</p>
      <div class="dog-quote-box">
      <p class="dog-quote">{{ dogQuote }}</p>
      </div>
      <img :src="dogImage" alt="Dog" class="dog-image" />
  
      <div v-if="challenges.length === 0" class="empty-message">No challenges today</div>
  
      <div v-for="(challenge, index) in challenges" :key="index" class="challenge-card">
        <label>
          <input type="checkbox" v-model="challenge.completed" @change="updateDogMood" />
          {{ challenge.name }}
        </label>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        challenges: [
          { name: 'Eat at least one apple', completed: false },
          { name: 'Do 10 pushups', completed: false },
          { name: 'Cook one meal from the recipes', completed: false },
        ],
      };
    },
    computed: {
      dogImage() {
        const completed = this.challenges.filter((c) => c.completed).length;
        if (completed === this.challenges.length && completed > 0) return '/dog-happy.png';
        if (completed / this.challenges.length < 1 / 3 || this.challenges.length === 0) return '/dog-angry.png';
        if (completed / this.challenges.length < 2 / 3) return '/dog-sad.png';
        if (completed / this.challenges.length < 1) return '/dog-neutral.png';
      },
      dogQuote() {
        const completed = this.challenges.filter((c) => c.completed).length;
        if (completed === this.challenges.length && completed > 0) return "You did it! You're pawsome!";
        if (completed / this.challenges.length < 1 / 3 || this.challenges.length === 0) return "Woof... you're so lazy!";
        if (completed / this.challenges.length < 2 / 3) return "Tail's wagging... you can do better!";
        if (completed / this.challenges.length < 1) return "Almost there pal!";
      },
    },
    methods: {
      updateDogMood() {
        // Trigger reactivity
        this.challenges = [...this.challenges];
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
    text-align:left;
  }
  .dog-image {
    width: 200px;
    margin: 5px auto;
  }
  .text {
    font-size: 12px;
    text-align:left;
    color: gray;
  }
  .quote {
    font-style: italic;
    font-size: 20px;
  }
  .challenge-card input[type="checkbox"] {
  border-radius: 0%;
  width: 14px;
  height: 14px;
  accent-color: #007aff;
    }
  .challenge-card {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  .empty-message {
    margin-top: 2rem;
    color: gray;
  }
  .dog-quote-box {
  max-width: 260px;
  margin: 10px auto;
  background-color: sandybrown;
  border: 2px solid saddlebrown;
  border-radius: 20px;
  padding: 10px 15px;
  position: relative;
  font-style: italic;
}

.dog-quote-box::after {
  content: '';
  position: absolute;
  bottom: -15px;
  left: 30px;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-top-color: sandybrown;
  border-bottom: 0;
  margin-left: -10px;
}
  </style>
  