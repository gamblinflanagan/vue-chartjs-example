<script setup>
defineProps({
  name: {
    type: String,
    required: true,
  },
})
</script>

<script>
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

const startYear = 2000
const endYear = 2025

let label = []
let timeframe = []

export default {
  name: 'LineChart',
  components: { Line },
  data() {
    return {
      chartData: {
        labels: label,
        datasets: [
          {
            label: 'Line Dataset',
            data: timeframe,
            fill: false,
            borderColor: 'rgb(75, 192, 192)',
            tension: 0.1,
          },
        ],
      },
    }
  },
  //   data: () => ({
  //     loaded: false,
  //     chartData: null,
  //   }),
  //   async mounted() {
  //     this.loaded = false

  //     try {
  //       const { userlist } = await fetch(
  //         'https://fed-charts-default-rtdb.firebaseio.com/observations.json',
  //       )
  //       this.chartdata = userlist

  //       this.loaded = true
  //     } catch (e) {
  //       console.error(e)
  //     }
  //   },
}

async function MakeRequestHandler() {
  try {
    const response = await fetch(
      'https://fed-charts-default-rtdb.firebaseio.com/observations.json',
      {
        //requestConfig.url, {
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
  } catch (err) {
    console.log('THE REQUEST FAILED', err)
  }
  for (let i = startYear; i < endYear + 1; i++) {
    label.push(i)
  }
  console.log(label)
}
</script>

<template>
  <button v-on:click="MakeRequestHandler">make request</button>
  <input type="text" id="fname" name="fname" value="2000-01-01" />
  <input type="text" id="lname" name="lname" value="2025-01-01" /><br /><br />
  <!-- <h1>this is the bar chart: {{ name }}</h1> -->
  <br />

  <Line :data="chartData" :options="chartOptions" />
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
