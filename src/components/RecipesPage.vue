<template>
    <ul>
      <li v-for="recipe in recipes" :key="recipe.id" style="background: #f9f9f9; padding: 0.5rem; margin-bottom: 0.5rem; border-radius: 5px;">
        <strong>{{ recipe.title }}</strong>
        <p style="font-size: 0.9rem; color: #555;">{{ recipe.ingredients }}</p>
      </li>
    </ul>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const recipes = ref([
    { id: 1, title: 'Greek Yogurt Bowl', ingredients: 'Yogurt, berries, honey' },
    { id: 2, title: 'Avocado Toast', ingredients: 'Avocado, bread, salt' },
    { id: 3, title: 'Oatmeal', ingredients: 'Oats, milk, banana' },
  ])
  </script>
  