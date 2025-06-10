<template>
  <div class="recipes-page-container">
    <transition :name="transitionDirection">
      <div v-if="currentView === 'main'" key="main" class="view-content main-recipes-view">
        <div class="header main-header">
          <h1 class="page-title">Mom's recipes</h1>
        </div>

        <div class="recipe-category-list-wrapper"> 
          <div class="category-item" @click="goToBasicRecipes">
            <span class="category-name">Basic recipes</span>
            <span class="arrow-icon">く</span>
          </div>
          <div class="category-item" @click="showUnlockRecipesAlert = true">
            <span class="category-name">Recipes won</span>
            <span class="arrow-icon">く</span>
          </div>
        </div>

        <div class="illustration-wrapper">
          <img src="/recipes/kitchen-illustration.png" alt="Kitchen illustration" class="kitchen-illustration" />
        </div>

        <div v-if="showUnlockRecipesAlert" class="alert-overlay">
          <div class="alert-content">
            <p class="alert-title">Complete more habits to unlock new recipes!</p>
            <p class="alert-subtitle">Don't cheat, your mom will be very mad!</p>
            <div class="alert-actions">
              <button @click="goToHabits" class="alert-button go-to-habits">Go to habits</button>
              <button @click="closeAlert" class="alert-button got-it">Got it!</button>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="currentView === 'basicList'" key="basicList" class="view-content basic-recipes-list-view">
        <div class="header subview-header">
          <div class="header-top-row">
            <button @click="goBack" class="back-button">く</button>
            <span class="previous-page-title">{{ previousPageTitle }}</span>
          </div>
          <h1 class="page-title current-page-sub-title">Basic recipes</h1>
        </div>

        <ul class="recipes-list-scrollable" ref="recipesList">
          <li v-for="(recipe, i) in basicRecipes" :key="recipe.id" class="recipe-list-item" @click="selectRecipe(recipe)">
            <img :src="recipe.image" :alt="recipe.name" class="recipe-list-image" />
            <div class="recipe-list-details">
              <h2 class="recipe-list-name">{{ recipe.name }}</h2>
              <p class="recipe-list-subtitle">{{ recipe.subtitle }}</p>
            </div>
            <p class="details-text">Details</p>
            <span class="details-link">く </span>
          </li>
        </ul>

        <p class="note-text-wrapper">Note that these are just to get you started, make sure to complete habits to get better ones</p>
      </div>

      <div v-else-if="currentView === 'singleRecipe'" key="singleRecipe" class="view-content single-recipe-view">
        <div class="header subview-header">
          <div class="header-top-row">
            <button @click="goBack" class="back-button">く</button>
            <span class="previous-page-title">{{ previousPageTitle }}</span>
          </div>
          <h1 class="page-title current-page-sub-title">{{ selectedRecipe.name }}</h1>
        </div>

        <div class="single-recipe-scrollable-content">
          <div class="recipe-visual-section">
            <div class="recipe-image-wrapper">
              <img :src="selectedRecipe.image" :alt="selectedRecipe.name" class="single-recipe-image" />
            </div>
            <div class="recipe-info-tags-group">
              <span class="info-tag"><b>Serves: </b>{{ selectedRecipe.serves }}</span>
              <span class="info-tag" :class="getDifficultyTagClass(selectedRecipe.difficulty)"><b>Difficulty:</b> {{ selectedRecipe.difficulty }}</span>
              <span class="info-tag"><b>Prep time:</b> {{ selectedRecipe.time }} minutes</span>
            </div>
          </div>

          <div class="recipe-section ingredients-widget">
            <h3 class="section-title">Ingredients</h3>
            <div class="section-separator" :class="getSeparatorColorClass(selectedRecipe.difficulty)"></div>
            <ul class="ingredients-list">
                <li v-for="(ingredient, i) in selectedRecipe.ingredients" :key="i">{{ ingredient }}</li>
            </ul>
          </div>

          <div class="recipe-section method-widget">
            <h3 class="section-title">Method</h3>
            <div class="section-separator" :class="getSeparatorColorClass(selectedRecipe.difficulty)"></div>
            <p class="method-text">{{ selectedRecipe.method }}</p>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>
  
<script>
  export default {
    emits: ['change-tab'],
    data() {
      return {
        currentView: 'main', // 'main', 'basicList', 'singleRecipe'
        selectedRecipe: null,
        showUnlockRecipesAlert: false,
        previousPageTitle: '', 
        transitionDirection: 'slide-left', // 'slide-left' or 'slide-right' for animation direction
        savedScrollPosition: 0, // To save scroll position of recipe list
        basicRecipes: [
          {
            id: 1,
            name: "Caprese salad",
            subtitle: "Tomato and mozzarella salad",
            image: "/recipes/caprese-salad.png",
            serves: 4,
            difficulty: "Easy",
            time: 10,
            ingredients: [
              "8 plum ripe tomatoes",
              "2 balls of mozzarella",
              "handful of basil leaves",
              "2 spoons of olive oil and vinegar"
            ],
            method: "Arrange the slices of tomato and mozzarella on a platter, alternating between slices of each, and sprinkle over the basil leaves. Season generously with salt and black pepper, then drizzle over the olive oil and vinegar. Serve straightaway."
          },
          {
            id: 2,
            name: "Lasagne soup",
            subtitle: "Classic lasagna with a twist",
            image: "/recipes/lasagne-soup.png",
            serves: 6,
            difficulty: "Medium",
            time: 45,
            ingredients: [
              "1 tbsp olive oil",
              "1 onion, chopped",
              "2 cloves garlic, minced",
              "400g minced beef",
              "800g canned chopped tomatoes",
              "700ml beef stock",
              "1 tsp dried oregano",
              "Lasagne sheets, broken into pieces",
              "Ricotta cheese, for serving",
              "Fresh basil, for serving"
            ],
            method: "Heat oil in a large pot. Add onion and cook until softened. Add garlic and beef, cook until browned. Stir in tomatoes, stock, and oregano. Bring to a boil, then add broken lasagne sheets. Simmer for 15-20 minutes, or until pasta is tender. Serve with dollops of ricotta and fresh basil."
          },
          {
            id: 3,
            name: "Pizza margherita",
            subtitle: "To impress your friends",
            image: "/recipes/pizza-margherita.png",
            serves: 2,
            difficulty: "Hard",
            time: 60,
            ingredients: [
              "250g strong bread flour",
              "7g dried yeast",
              "1 tsp sugar",
              "1 tsp salt",
              "150ml warm water",
              "1 tbsp olive oil",
              "125g mozzarella cheese",
              "100g tomato passata",
              "Fresh basil leaves"
            ],
            method: "Mix flour, yeast, sugar, and salt. Add water and olive oil, knead for 10 mins. Let rise for 1 hour. Roll out dough, spread passata, tear mozzarella, bake at 220C for 10-12 mins. Garnish with basil."
          },
          {
            id: 4,
            name: "Cotoletta",
            subtitle: "Breaded chicken",
            image: "/recipes/cotoletta.png",
            serves: 2,
            difficulty: "Easy",
            time: 25,
            ingredients: [
              "2 chicken breasts",
              "1 egg, beaten",
              "50g breadcrumbs",
              "Salt and pepper",
              "Olive oil for frying"
            ],
            method: "Flatten chicken breasts. Dip in egg, then breadcrumbs. Season with salt and pepper. Fry in olive oil until golden brown and cooked through. Serve with lemon wedges."
          },
          {
            id: 5,
            name: "Strawberry panna cotta",
            subtitle: "A quick and light dessert",
            image: "/recipes/strawberry-panna-cotta.png",
            serves: 4,
            difficulty: "Easy",
            time: 20, // Prep time
            ingredients: [
              "400ml double cream",
              "50g caster sugar",
              "1 vanilla pod, split",
              "2 gelatine leaves",
              "200g strawberries, halved"
            ],
            method: "Soak gelatine in cold water. Heat cream, sugar, and vanilla in a pan until sugar dissolves. Squeeze gelatine, add to cream, stir until dissolved. Remove vanilla pod. Pour into ramekins, chill for 4 hours. Serve with strawberries."
          }
        ]
      };
    },
    methods: {
      resetToMainView() {
        if (this.currentView !== 'main') {
          this.transitionDirection = 'slide-right';
          this.currentView = 'main';
          this.selectedRecipe = null;
          this.previousPageTitle = '';
          this.savedScrollPosition = 0; // Reset scroll position when going to main
        }
      },
      goBack() {
        if (this.currentView === 'singleRecipe') {
          this.transitionDirection = 'slide-right';
          this.currentView = 'basicList';
          this.selectedRecipe = null; 
          this.previousPageTitle = 'Mom\'s recipes'; 
          this.$nextTick(() => { // Restore scroll position after DOM update
            if (this.$refs.recipesList && this.savedScrollPosition !== 0) {
              this.$refs.recipesList.scrollTop = this.savedScrollPosition;
            }
          });
        } else if (this.currentView === 'basicList') {
          this.transitionDirection = 'slide-right';
          this.currentView = 'main';
          this.previousPageTitle = ''; 
          this.savedScrollPosition = 0; 
        }
      },
      goToBasicRecipes() {
        this.transitionDirection = 'slide-left';
        this.currentView = 'basicList';
        this.previousPageTitle = 'Mom\'s recipes';
      },
      selectRecipe(recipe) {
        // Save scroll position before navigating away
        if (this.$refs.recipesList) {
          this.savedScrollPosition = this.$refs.recipesList.scrollTop;
        }
  
        this.transitionDirection = 'slide-left';
        this.selectedRecipe = recipe;
        this.currentView = 'singleRecipe';
        this.previousPageTitle = 'Basic recipes';
      },
      closeAlert() {
        this.showUnlockRecipesAlert = false;
      },
      goToHabits() {
        this.$emit('change-tab', 'habits');
        this.closeAlert();
      },
      getDifficultyTagClass(difficulty) {
        switch (difficulty.toLowerCase()) {
          case 'easy':
            return 'tag-easy';
          case 'medium':
            return 'tag-medium';
          case 'hard':
            return 'tag-hard';
          default:
            return '';
        }
      },
      getSeparatorColorClass(difficulty) {
        switch (difficulty.toLowerCase()) {
          case 'easy':
            return 'separator-easy';
          case 'medium':
            return 'separator-medium';
          case 'hard':
            return 'separator-hard';
          default:
            return 'separator-default'; // Fallback color
        }
      }
    }
  };
</script>
  
<style scoped>
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .recipes-page-container {
    padding: 0;
    height: 100%;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    position: relative;
  }
  
  .view-content {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: 0; 
    display: flex;
    flex-direction: column;
    background-color: white;
    box-sizing: border-box;
  }
  
  .slide-left-enter-active, .slide-left-leave-active,
  .slide-right-enter-active, .slide-right-leave-active {
    transition: transform 0.3s ease-in-out;
  }
  
  .slide-left-enter-from {
    transform: translateX(100%);
  }
  .slide-left-leave-to {
    transform: translateX(-100%);
  }
  
  .slide-right-enter-from {
    transform: translateX(-100%);
  }
  .slide-right-leave-to {
    transform: translateX(100%);
  }
  
  .header {
    flex-shrink: 0; 
    padding: 1rem 0rem 0;
    margin-bottom: 1rem; 
  }
  
  .main-header .page-title {
    text-align: left;
  }
  
  .subview-header {
    display: flex;
    flex-direction: column; 
    align-items: flex-start; 
  }
  
  .header-top-row {
    display: flex;
    align-items: center;
    width: 100%;
    margin-bottom: 0.25rem;
  }
  
  .back-button {
    background: none;
    border: none;
    font-size: 1.2rem;
    color: #007AFF;
    cursor: pointer;
    padding: 0;
    margin-right: 0.25rem;
  }
  
  .previous-page-title {
    color: #007AFF;
    font-size: 1rem;
    white-space: nowrap;
    line-height: 1;
  }
  
  .page-title {
    font-size: 28px;
    font-weight: bold;
    color: #000;
    text-align: left; 
    width: 100%;
  }
  
  .current-page-sub-title {
    font-size: 1.75rem;
    font-weight: bold;
    color: #000;
    text-align: left;
  }
  
  .main-recipes-view {

  }
  
  .recipe-category-list-wrapper {
    margin-bottom: 1.5rem;
  }

  .category-name {
    font-size: 1.2rem; 
    font-weight: 550; 
    color: #333; 
  }
  
  .category-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 0;
    border-bottom: 1px solid #eee;
    cursor: pointer;
    font-size: 1.1rem;
    color: #000;
  }
  
  .category-item:first-child {
    border-top: 1px solid #eee;
  }
  
  .arrow-icon {
    font-size: 1.25rem;
    color: #a0a0a0;
    transform: scale(-1, 1);
  }
  
  .illustration-wrapper {
    flex-grow: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .kitchen-illustration {
    max-width: 95%;
    height: auto;
    display: block;
  }
  
  .basic-recipes-list-view {
  
  }
  
  .recipes-list-scrollable {
    flex-grow: 1; 
    overflow-y: hidden; 
    -webkit-overflow-scrolling: touch; 
    margin-top: 0; 
  }
  
  .recipe-list-item {
    display: flex;
    align-items: center;
    border-bottom: 1px solid #eee;
    cursor: pointer;
  }
  
  .recipe-list-image {
    width: 70px; 
    height: 70px; 
    border-radius: 8px;
    object-fit: cover;
    margin-right: 1rem; 
    flex-shrink: 0;
  }
  
  .recipe-list-details {
    flex-grow: 1;
  }
  
  .recipe-list-name {
    font-size: 1rem; 
    font-weight: 500;
    margin-bottom: 0;
    line-height: 1;
  }
  
  .recipe-list-subtitle {
    font-size: 0.8rem; 
    color: #888;
    line-height: 1.8;
  }
  
  .details-text {
    font-size: 1rem; 
    color: #a0a0a0;
    margin-left: 0.5rem; 
    white-space: nowrap;
    flex-shrink:0;
  }

  .details-link {
    font-size: 1rem; 
    color: #a0a0a0;
    margin-left: 0.5rem;
    white-space: nowrap;
    flex-shrink: 0;
    transform: scale(-1, 1); 
  }
  
  .note-text-wrapper {
    font-size: 0.75rem;
    color: #888;
    text-align: left;
    flex-shrink: 0;
  }
  
  .single-recipe-view {
    overflow-y: hidden; 
  }
  
  .single-recipe-scrollable-content {
    flex-grow: 1;
    overflow-y: auto; 
    -webkit-overflow-scrolling: touch;
    padding: 0 1rem 1rem; 
  }
  
  .recipe-visual-section {
    display: flex;
    align-items: flex-start; 
    gap: 1rem; 
    margin-bottom: 1.5rem;
  }
  
  .recipe-image-wrapper {
    width: 120px; 
    height: 120px; 
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    flex-shrink: 0; 
  }
  
  .single-recipe-image {
    width: 100%;
    height: 100%;
    object-fit: cover; 
    display: block;
  }
  
  .recipe-info-tags-group { 
    display: flex;
    flex-wrap: wrap;
    align-content: flex-start; 
    gap: 0.5rem;
    flex-grow: 1; 
  }
  
  .info-tag {
    background-color: #F0F0F0;
    color: #000;
    padding: 0.4rem 0.8rem;
    border-radius: 20px;
    font-size: 0.85rem;
    white-space: nowrap;
  }

  .info-tag-text {
    font-weight: bold;
  }
  
  .info-tag.tag-easy {
    background-color: #e6ffe6;
    color: #28a745;
  }
  .info-tag.tag-medium {
    background-color: #fff3cd;
    color: #fd7e14;
  }
  .info-tag.tag-hard {
    background-color: #ffe6e6;
    color: #dc3545;
  }
  
  .ingredients-widget, .method-widget {
    background-color: #F8F8F8;
    border-radius: 12px;
    padding: 1rem;
    margin-bottom: 1.5rem;
    box-shadow: 0 1px 4px rgba(0,0,0,0.05);
    box-sizing: border-box;
  }
  
  .section-title {
    font-size: 1.1rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
    text-align: left;
  }
  
  .section-separator {
    width: 40px;
    height: 3px;
    margin-bottom: 1rem;
    border-radius: 1.5px;
    background-color: #FF69B4; 
  }
  
  .section-separator.separator-easy {
    background-color: #28a745; 
  }
  .section-separator.separator-medium {
    background-color: #fd7e14; 
  }
  .section-separator.separator-hard {
    background-color: #dc3545;
  }
  
  .ingredients-list {
    list-style: none;
    padding: 0;
    margin: 0;
    font-size: 0.95rem;
    color: #333;
    line-height: 1.4;
  }
  
  .ingredients-list li {
    padding: 0.25rem 0;
    border-bottom: 1.5px dashed #eee;
  }
  .ingredients-list li:last-child {
    border-bottom: none;
  }
  
  .method-text {
    font-size: 0.95rem;
    color: #333;
    line-height: 1.5;
    text-align: left;
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
  }
  
  .alert-content {
    background: #fefefe;
    padding: 1rem;
    width: 100%;
    max-width: 300px;
    max-height: 140px;
    border-radius: 20px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  }
  
  .alert-title {
    font-size: 16px;
    font-weight: 600; 
    text-align: center;
    margin-bottom: 0.5rem;
    color: #000;
  }
  
  .alert-subtitle {
    font-size: 12px;
    text-align: center;
    color: #666;
    margin-bottom: 1.5rem;
  }
  
  .alert-actions {
    display: flex;
    flex-direction: row; 
    border-top: 1px solid #eee; 
  }
  
  .alert-button {
    background: none;
    border: none;
    padding: 1rem;
    font-size: 1.05rem;
    cursor: pointer;
    width: 100%; 
    text-align: center;
  }
  
  .alert-button.go-to-habits {
    color: #007AFF; 
    font-weight: 600; 
  }
  
  .alert-button.got-it {
    color: #007AFF;
    font-weight: normal; 
    border-left: 1px solid #eee; 
  }
</style>