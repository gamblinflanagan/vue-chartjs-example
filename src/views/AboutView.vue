<script setup>
import BarChart from '../components/barChart.vue'
</script>

<template>
  <div class="about">
    <!-- <h1>This is the page to put the charts</h1> -->
    <!-- <BarChart v-for="i in 5" num1="5" /> -->
    <BarChart name="Bar chart component" />
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
