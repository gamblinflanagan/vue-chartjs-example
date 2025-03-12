
<!-- <script setup>
  defineProps({ chartUrl, chartName });
</script> -->
<script>
import { ref } from 'vue'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  BarElement,
  Title,
  Tooltip,
  Legend,
} from 'chart.js'
import { Line } from 'vue-chartjs'
// import * as chartConfig from './chartConfig.js'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  LineElement,
  CategoryScale,
  LinearScale,
  PointElement,
)

// const startYear = 1980
// const endYear = 2025

const label = ref([])
const timeframe = ref([])
let chartName = ''
let chartUrl = ''

// function MakeRequestHandler() {
//     for (let i = startYear; i < endYear + 1; i++) {
//       label.push(i)
//     }
//     console.log(label)
// }

export default {
  props: ['chartName', 'chartUrl'],
  setup(props) {
    // setup() receives props as the first argument.
    chartName = props.chartName,
    chartUrl = props.chartUrl
  },
  name: 'LineChart',
  components: { Line },
  data: () => ({
    loaded: false,
    chartData: {
      labels: label,
      datasets: [
        {
          label: chartName,
          data: timeframe,
          fill: false,
          borderColor: 'rgb(75, 192, 192)',
          tension: 0.1,
        },
      ],
    },
  }),
  async mounted() {
    this.loaded = false

    try {
      // console.log('CHARTNAME', this.chartName);
      // console.log('CHARTNAME', this.chartUrl);
      const response = await fetch(
        this.chartUrl,
        {
          method: 'GET',
          headers: {},
          body: null,
        },
      )

      if (!response.ok) {
        console.log('THE REQUEST FAILEd')
        throw new Error('Request failed!')
      }

      const data = await response.json()
      // console.log(data) //applyData(data);
      timeframe.value = data.map((val) => val.value)
      label.value = data.map((date) => date.date)//.substring(0, 4))

      // console.log(timeframe)
      // console.log(label)

      this.chartdata = {
        labels: label,
        datasets: [
          {
            label: chartName,
            data: timeframe,
            fill: false,
            borderColor: 'rgb(75, 192, 192)',
            tension: 0.1,
          },
        ],
      }

      this.loaded = true
    } catch (e) {
      console.error(e)
    }
  },
}
</script>

<template>
  <!-- <button @click="MakeRequestHandler">make request</button>
  <input type="text" id="fname" name="fname" value="2000-01-01" />
  <input type="text" id="lname" name="lname" value="2025-01-01" /><br /><br />
  <br /> -->
  <!-- <h1>{{ name }}</h1> -->

  <Line v-if="loaded" :data="chartData" />
</template>

<style scoped>

</style>
