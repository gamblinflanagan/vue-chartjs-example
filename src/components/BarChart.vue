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
// import { Bar } from 'vue-chartjs'
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

// export default {
//   name: 'BarChart',
//   components: { Bar },
//   data() {
//     return {
//       chartData: {
//         labels: [
//           'January',
//           'February',
//           'March',
//           'April',
//           'May',
//           'June',
//           'July',
//           'August',
//           'September',
//           'October',
//           'November',
//           'December',
//         ],
//         datasets: [
//           {
//             label: 'Bar Dataset',
//             data: [65, 59, 80, 81, 56, 55, 40],
//             backgroundColor: [
//               'rgba(255, 99, 132, 0.2)',
//               'rgba(255, 159, 64, 0.2)',
//               'rgba(255, 205, 86, 0.2)',
//               'rgba(75, 192, 192, 0.2)',
//               'rgba(54, 162, 235, 0.2)',
//               'rgba(153, 102, 255, 0.2)',
//               'rgba(201, 203, 207, 0.2)',
//             ],
//             borderColor: [
//               'rgb(255, 99, 132)',
//               'rgb(255, 159, 64)',
//               'rgb(255, 205, 86)',
//               'rgb(75, 192, 192)',
//               'rgb(54, 162, 235)',
//               'rgb(153, 102, 255)',
//               'rgb(201, 203, 207)',
//             ],
//             borderWidth: 1,
//           },
//         ],
//       },
//     }
//   },
// }
export default {
  name: 'LineChart',
  components: { Line },
  data() {
    return {
      chartData: {
        labels: [
          'January',
          'February',
          'March',
          'April',
          'May',
          'June',
          'July',
          'August',
          'September',
          'October',
          'November',
          'December',
        ],
        datasets: [
          {
            label: 'Line Dataset',
            data: [65, 59, 80, 81, 56, 55, 40, 45, 69, 88, 80, 95],
            fill: false,
            borderColor: 'rgb(75, 192, 192)',
            tension: 0.1,
          },
        ],
      },
    }
  },
}
</script>

<template>
  <!-- <h1>this is the bar chart: {{ name }}</h1> -->
  <br />
  <div>
    <Line :data="chartData" :options="chartOptions" />
  </div>
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
