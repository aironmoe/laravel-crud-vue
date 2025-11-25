<script setup lang="ts">
import { computed } from 'vue'
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import { type PropType } from 'vue'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

const props = defineProps({
  monthlyDone: {
    type: Array as PropType<number[]>,
    required: true,
  },
  monthlyPending: {
    type: Array as PropType<number[]>,
    required: true,
  }
})

const monthLabels = [
  'January','February','March','April','May','June',
  'July','August','September','October','November','December'
]

const chartData = computed(() => ({
  labels: monthLabels,
  datasets: [
    {
      label: 'Done',
      data: props.monthlyDone,
      backgroundColor: '#16a34a',
    },
    {
      label: 'Pending',
      data: props.monthlyPending,
      backgroundColor: '#f59e0b',
    }
  ]
}))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: { position: 'top' },
    title: { display: true, text: 'Monthly Tasks Summary' }
  },
  scales: {
    y: { beginAtZero: true }
  }
}
</script>

<template>
  <Bar
    :data="chartData"
    :options="chartOptions"
  />
</template>
