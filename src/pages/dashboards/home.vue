
<script setup lang="ts">
import { ref, computed } from 'vue'

import { useTheme } from 'vuetify'
import { hexToRgb } from '@layouts/utils'

// Get Vuetify theme
const vuetifyTheme = useTheme()

// Chart series
const series = [
  {
    name: 'Sales',
    data: [32, 27, 27, 30, 25, 25],
  },
  {
    name: 'Visits',
    data: [25, 35, 20, 20, 20, 20],
  },
]

// Chart options (dynamic theme)
const chartOptions = computed(() => {
  const currentTheme = vuetifyTheme.current.value.colors
  const variableTheme = vuetifyTheme.current.value.variables

  const borderColor = `rgba(${hexToRgb(String(variableTheme['border-color']))},${variableTheme['border-opacity']})`
  const labelColor = `rgba(${hexToRgb(currentTheme['on-surface'])},${variableTheme['disabled-opacity']})`
  const legendColor = `rgba(${hexToRgb(currentTheme['on-background'])},${variableTheme['medium-emphasis-opacity']})`

  return {
    chart: {
      type: 'line',
      toolbar: { show: false },
    },
    stroke: {
      show: true,      // enable stroke
      width: 2,        // line thickness
      curve: 'smooth', // makes it nice and rounded
    },
    legend: {
      show: true,
      position: 'bottom',
      fontSize: '13px',
      labels: { colors: legendColor },
      markers: { width: 12, height: 12, offsetX: -8 },
      itemMargin: { horizontal: 10 },
    },
    colors: [currentTheme.primary, currentTheme.info],
    fill: { opacity: [1, 0.85] },
    markers: { size: 0 },
    grid: { show: false, padding: { top: 0, bottom: -5 } },
    xaxis: {
      categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
      labels: {
        style: {
          colors: Array(6).fill(labelColor),
          fontSize: '13px',
          fontFamily: 'Public Sans',
        },
      },
    },
    yaxis: { show: false, min: 0, max: 40, tickAmount: 4 },
    responsive: [
      {
        breakpoint: 769,
        options: { chart: { height: 372 } },
      },
    ],
  }
})

// Action menu
const moreList = [
  { title: 'View More', value: 'View More' },
  { title: 'Delete', value: 'Delete' },
]

// Example stats
const stats = ref([
  { title: 'Users', value: 1245, icon: 'mdi-account', color: 'primary' },
  { title: 'Revenue', value: '$12,450', icon: 'mdi-currency-usd', color: 'success' },
  { title: 'Orders', value: '320', icon: 'mdi-cart-outline', color: 'warning' },
])
</script>

<template>
  <VContainer
    fluid
    class="home--page"
  >
    <VRow>
      <!-- Stats Cards -->
      <VCol
        v-for="stat in stats"
        :key="stat.title"
        cols="12"
        md="4"
      >
        <VCard
          class="pa-4"
          :color="`${stat.color}-lighten-4`"
          flat
        >
          <VRow align="center">
            <VCol cols="3">
              <VIcon
                :color="stat.color"
                size="36"
              >
                {{ stat.icon }}
              </VIcon>
            </VCol>
            <VCol>
              <h4 class="text-subtitle-2">
                {{ stat.title }}
              </h4>
              <h2 class="font-weight-bold">
                {{ stat.value }}
              </h2>
            </VCol>
          </VRow>
        </VCard>
      </VCol>
    </VRow>

    <VRow>
      <!-- Sales Chart -->
      <VCol
        cols="12"
        md="8"
      >
        <VCard>
          <VCardItem class="pb-4">
            <VCardTitle>Sales Overview</VCardTitle>
            <VCardSubtitle>Last 6 Months</VCardSubtitle>

            <template #append>
              <div class="mt-n4 me-n2">
                <MoreBtn
                  size="small"
                  :menu-list="moreList"
                />
              </div>
            </template>
          </VCardItem>

          <VCardText>
            <VueApexCharts
              :options="chartOptions"
              :series="series"
              height="290"
            />
          </VCardText>
        </VCard>
      </VCol>

      <!-- Recent Orders -->
      <VCol
        cols="12"
        md="4"
      >
        <VCard class="pa-4">
          <h3 class="text-h6 mb-4">
            Recent Orders
          </h3>
          <VTable>
            <thead>
              <tr>
                <th>Order</th>
                <th>Status</th>
                <th>Amount</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="i in 5"
                :key="i"
              >
                <td>#{{ 1000 + i }}</td>
                <td>
                  <VChip
                    color="success"
                    size="small"
                  >
                    Completed
                  </VChip>
                </td>
                <td>${{ (i * 100).toFixed(2) }}</td>
              </tr>
            </tbody>
          </VTable>
        </VCard>
      </VCol>
    </VRow>
  </VContainer>
</template>
