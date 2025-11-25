<script setup lang="ts">
import { Line } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title, 
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import { type PropType } from 'vue'

// define props
const props = defineProps({
    title: String,
    titleMessage: String,
    titlecolor: String,
    titlemonths: {
        type: Array,
        required: true
    }
})

// Register Chart.js components
ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale)

// Define your chart data and options
const chartData = {
  labels: props.titlemonths,
  datasets: [
    {
      label: props.title,
      data: [11, 89, 99, 110, 20, 15,],
      borderColor: props.titlecolor,
      backgroundColor: 'rgba(66, 165, 245, 0.2)',
      tension: 0.5,
    },
  ],
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: { position: 'top' as const },
    title: { display: true, text: props.titleMessage},
  },
}

</script>

<template>
  <div style="height: 400px">
    <Line :data="chartData" :options="chartOptions" />
  </div>
</template>
