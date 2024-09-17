<script setup lang="ts">
import { ref, computed } from 'vue';

const select = ref({ state: 'Hoje', abbr: 'FL' });
const items = [
  { state: 'Hoje', abbr: 'FL' },
  { state: 'Este Mês', abbr: 'GA' },
  { state: 'Este Ano', abbr: 'NE' }
];

// chart 1
const chartOptions1 = computed(() => {
  return {
    chart: {
      type: 'bar',
      height: 480,
      fontFamily: `inherit`,
      foreColor: '#a1aab2',
      stacked: true
    },
    colors: ['#008000', '#1e88e5', '#5e35b1', '#ede7f6'],
    responsive: [
      {
        breakpoint: 480,
        options: {
          legend: {
            position: 'bottom',
            offsetX: -10,
            offsetY: 0
          }
        }
      }
    ],
    plotOptions: {
      bar: {
        horizontal: false,
        columnWidth: '50%'
      }
    },
    xaxis: {
      type: 'category',
      categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
    },
    legend: {
      show: true,
      fontFamily: `'Roboto', sans-serif`,
      position: 'bottom',
      offsetX: 20,
      labels: {
        useSeriesColors: false
      },
      markers: {
        width: 16,
        height: 16,
        radius: 5
      },
      itemMargin: {
        horizontal: 15,
        vertical: 8
      }
    },
    fill: {
      type: 'solid'
    },
    dataLabels: {
      enabled: false
    },
    grid: {
      show: true
    },
    tooltip: {
      theme: 'dark'
    }
  };
});

// chart 1
const lineChart1 = {
  series: [
    {
      name: 'Pagamento Realizado',
      data: [35, 125, 35, 35, 35, 80, 35, 20, 35, 45, 15, 75]
    },
    {
      name: 'Em Andamento',
      data: [35, 15, 15, 35, 65, 40, 80, 25, 15, 85, 25, 75]
    },
  ]
};
</script>

<template>
  <v-card elevation="0">
    <v-card variant="outlined">
      <v-card-text>
        <v-row>
          <v-col cols="12" sm="9">
            <span class="text-subtitle-2 text-disabled font-weight-bold">Total Associados Por Mês</span>
            <h3 class="text-h3 mt-1">324</h3>
          </v-col>
          <v-col cols="12" sm="3">
            <v-select
              color="primary"
              variant="outlined"
              hide-details
              v-model="select"
              :items="items"
              item-title="state"
              item-value="abbr"
              label="Select"
              persistent-hint
              return-object
              single-line
            >
            </v-select>
          </v-col>
        </v-row>
        <div class="mt-4">
          <apexchart type="bar" height="480" :options="chartOptions1" :series="lineChart1.series"> </apexchart>
        </div>
      </v-card-text>
    </v-card>
  </v-card>
</template>
