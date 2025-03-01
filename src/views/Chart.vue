<script setup>
  import { ref } from 'vue'
  import LineChart from '../components/lineChart.vue'

  const charts = ref([
    {
      name: 'Global M2',
      link: 'https://fed-charts-default-rtdb.firebaseio.com/WM2NS/observations.json',
    },
    {
      name: 'Reverse Repo Market',
      link: 'https://fed-charts-default-rtdb.firebaseio.com/RRPONTSYD/observations.json',
    },
  ]);
</script>

<template>
  <div v-for="(chart, index) in charts" v-bind:key="index">
    <LineChart :chartName="chart.name" :chartUrl="chart.link" />
    <br />
  </div>
</template>

<style>

</style>
