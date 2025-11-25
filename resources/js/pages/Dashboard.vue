<script setup lang="ts">
import { ref, onMounted } from 'vue'
import AppLayout from '@/layouts/AppLayout.vue'
import { dashboard } from '@/routes'
import { type BreadcrumbItem } from '@/types'
import { Head } from '@inertiajs/vue3'
import PlaceholderPattern from '../components/PlaceholderPattern.vue'
import LineChart from '@/components/LineChart.vue'
import BarChart from '@/components/BarChart1.vue'

const breadcrumbs: BreadcrumbItem[] = [
    { title: 'Dashboard', href: dashboard().url },
]

// Line chart variables
const title = ref('Profit')
const stocks = ref('Stocks') 
const ratbu = ref('Inventory')
const burnik = ref('Sales')
const chart2color = ref('#06D63A')
const monthlyDone = ref([15, 61, 70, 76, 72, 11, 3, 13, 7, 9, 0, 0])
const monthlyPending = ref([5, 10, 8, 12, 9, 3, 1, 4, 2, 2, 0, 0])
const months = ref(['Jan','Feb','Mar','Apr','May','Jun','Jul'])

// Pending items list
const pendingItems = ref([
  { name: 'Order #001', amount: '$120' },
  { name: 'Order #002', amount: '$85' },
  { name: 'Order #003', amount: '$230' },
  { name: 'Order #004', amount: '$50' },
])

// Reset chart at start of new year
onMounted(() => {
  const today = new Date()
  const month = today.getMonth() 

  if (month === 0) {
    monthlyDone.value = Array(12).fill(0)
    monthlyPending.value = Array(12).fill(0)
  }
})
</script>

<template>
<Head title="Dashboard" />

<AppLayout :breadcrumbs="breadcrumbs">
    <div class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4">

        <!-- Top 3 Line Charts -->
        <div class="grid auto-rows-min gap-4 md:grid-cols-3">
            <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border p-4">
                <h1 class="text-2xl font-bold mb-4">Inventory</h1>
                <LineChart :title="title" :titleMessage="ratbu" :titlecolor="chart2color" :titlemonths="months" />
            </div>

            <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border p-4">
                <h1 class="text-2xl font-bold mb-4">Sales</h1>
                <LineChart :title="stocks" :titleMessage="burnik" :titlecolor="chart2color" :titlemonths="months"/>
            </div>

            <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border p-4">
                <PlaceholderPattern />
            </div>
        </div>

        <!-- Bottom: Pending List & BarChart side by side -->
        <div class="flex gap-4">

            <!-- Left: Pending List -->
            <div class="w-1/3 bg-white dark:bg-gray-800 p-4 rounded-xl border border-sidebar-border/70 overflow-auto">
                <h2 class="text-xl font-bold mb-4">Pending Transactions</h2>
                <ul class="space-y-2">
                    <li v-for="(item, index) in pendingItems" :key="index" class="p-2 border rounded hover:bg-gray-100 dark:hover:bg-gray-700">
                        {{ item.name }} - {{ item.amount }}
                    </li>
                </ul>
            </div>

            <!-- Right: BarChart -->
            <div class="w-2/3 h-[300px] p-4 rounded-xl border border-sidebar-border/70 bg-white dark:bg-gray-800">
                <BarChart :monthlyDone="monthlyDone" :monthlyPending="monthlyPending" />
            </div>

        </div>
    </div>
</AppLayout>
</template>
