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
        <input type="checkbox" class="circle" :checked="challenge.completed" @change="emitUpdateChallenge(index)" />
            {{ challenge.name }}
      </label>
    </div>
  </div>
</template>
  
<script>
  export default {
    props: {
      challenges: {
        type: Array,
        default: () => [],
      },
    },
    emits: ['update:challenges'], 
    data() {
      return {
      };
    },
    computed: {
      dogImage() {
        const completed = this.challenges.filter((c) => c.completed).length;
        if (completed === this.challenges.length && completed > 0) return '/challenges/dog-happy.png';
        if (completed / this.challenges.length < 1 / 3) return '/challenges/dog-angry.png';
        if (completed / this.challenges.length < 2 / 3) return '/challenges/dog-sad.png';
        if (completed / this.challenges.length < 1) return '/challenges/dog-neutral.png';
        return '/challenges/dog-angry.png'; 
      },
      dogQuote() {
        const completed = this.challenges.filter((c) => c.completed).length;
        if (completed === this.challenges.length && completed > 0) return "You did it! You're pawsome!";
        if (completed / this.challenges.length < 1 / 3) return "Woof... you're so lazy!";
        if (completed / this.challenges.length < 2 / 3) return "Tail's wagging... you can do better!";
        if (completed / this.challenges.length < 1) return "Almost there pal!";
        return "Woof...you're so lazy!";
      },
    },
    methods: {
      emitUpdateChallenge(index) {
        const updatedChallenges = [...this.challenges]; 
        updatedChallenges[index].completed = !updatedChallenges[index].completed;
        this.$emit('update:challenges', updatedChallenges); 
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
    text-align:left;
  }
  .dog-image {
    width: 200px;
    margin: 5px auto;
  }
  .text {
    font-size: 12px;
    font-weight: lighter;
    text-align:left;
    color: gray;
  }
  .quote {
    font-style: italic;
    font-size: 20px;
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
  