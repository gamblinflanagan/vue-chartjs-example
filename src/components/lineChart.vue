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

// function MakeRequestHandler() {
//     for (let i = startYear; i < endYear + 1; i++) {
//       label.push(i)
//     }
//     console.log(label)
// }

export default {
  props: ['chartName'],
  setup(props) {
    // setup() receives props as the first argument.
    chartName = props.chartName
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
      const response = await fetch(
        'https://fed-charts-default-rtdb.firebaseio.com/observations.json',
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
      console.log(data) //applyData(data);
      timeframe.value = data.map((val) => val.value)
      label.value = data.map((date) => date.date.substring(0, 4))

      console.log(timeframe)
      console.log(label)

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
.item {
  margin-top: 2rem;
  display: flex;
  position: relative;
}

.details {
  flex: 1;
  margin-left: 1rem;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;
  color: var(--color-text);
}

h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
  }

  i {
    top: calc(50% - 25px);
    left: -26px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }

  .item:before {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    bottom: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:after {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    top: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:first-of-type:before {
    display: none;
  }

  .item:last-of-type:after {
    display: none;
  }
}
</style>
