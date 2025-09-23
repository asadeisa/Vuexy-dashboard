<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { ofetch } from 'ofetch'
import { useTheme } from 'vuetify'
import { hexToRgb } from '@layouts/utils'

// Vuetify theme
const vuetifyTheme = useTheme()

// API data
interface Metric {
  title: string
  unit: string
  color: string
  currentValue: number
  differenceValue: number
  icon: string
}

const metrics = ref<Metric[]>([])
const loading = ref(true)

// Fetch data (with ~60s delay warning)
const fetchData = async () => {
  loading.value = true
  try {
    const res = await ofetch('https://api.fitcode.life/api/v1/test/data-1')
    metrics.value = res.data.map((item: any) => ({
      ...item,
      currentValue: Number(item.currentValue),
    }))
  } catch (err) {
    console.error(err)
  } finally {
    loading.value = false
  }
}

onMounted(fetchData)

// Chart data
const series = computed(() => [
  {
    name: 'Current Value',
    data: metrics.value.map(m => m.currentValue),
  },
])

const chartOptions = computed(() => {
  const currentTheme = vuetifyTheme.current.value.colors
  const variableTheme = vuetifyTheme.current.value.variables

  const labelColor = `rgba(${hexToRgb(currentTheme['on-surface'])},${variableTheme['disabled-opacity']})`
  const legendColor = `rgba(${hexToRgb(currentTheme['on-background'])},${variableTheme['medium-emphasis-opacity']})`

  return {
    chart: { type: 'bar', toolbar: { show: false } },
    plotOptions: { bar: { borderRadius: 6, columnWidth: '40%' } },
    legend: {
      show: true,
      position: 'bottom',
      labels: { colors: legendColor },
    },
    xaxis: {
      categories: metrics.value.map(m => m.title),
      labels: { style: { colors: Array(metrics.value.length).fill(labelColor) } },
    },
    yaxis: { show: true },
    colors: [currentTheme.primary],
  }
})
</script>

<template>
  <VContainer fluid>
    <VRow>
      <VCol
        v-for="metric in metrics"
        :key="metric.title"
        cols="12"
        sm="6"
        md="4"
      >
        <VCard class="pa-4" flat>
          <VRow align="center">
            <VCol cols="3">
              <VIcon :color="metric.color" size="36">
                {{ metric.icon }}
              </VIcon>
            </VCol>
            <VCol>
              <h4 class="text-subtitle-2">{{ metric.title }}</h4>
              <h2 class="font-weight-bold">
                {{ metric.currentValue }} {{ metric.unit }}
              </h2>
              <span class="text-caption text-medium-emphasis">
                Δ {{ metric.differenceValue }}
              </span>
            </VCol>
          </VRow>
        </VCard>
      </VCol>
    </VRow>

    <VRow>
      <!-- Chart -->
      <VCol cols="12">
        <VCard>
          <VCardItem>
            <VCardTitle>Health Metrics Overview</VCardTitle>
            <VCardSubtitle>Latest Recorded Values</VCardSubtitle>
          </VCardItem>

          <VCardText>
            <v-progress-circular
              v-if="loading"
              indeterminate
              color="primary"
              size="40"
            />
            <VueApexCharts
              v-else
              :options="chartOptions"
              :series="series"
              height="320"
            />
          </VCardText>
        </VCard>
      </VCol>
    </VRow>
  </VContainer>
</template>
