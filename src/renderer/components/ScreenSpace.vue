<template>
    <div class="container">
        <slot />
    </div>
  </template>
  
  <script setup lang="ts">
  </script>
  
  <style scoped>
  .container {
    display: grid;
    grid-template-columns: minmax(20ch, 25%) 1fr minmax(20ch, 25%);
    grid-template-rows: auto minmax(20ch, 33.33%);

    /* Ensure grid items stretch to fill cells */
    align-items: stretch; /* Vertical stretching */
    justify-items: stretch; /* Horizontal stretching */

    /* Styles to ensure it occupies full width and height of its parent */
    width: 100%;
    height: 100%;
    box-sizing: border-box;
  }
  </style>
  