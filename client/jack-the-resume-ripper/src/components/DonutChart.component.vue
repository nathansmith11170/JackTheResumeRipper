<template>
  <div class="example">
    <apexcharts width="500" height="350" type="donut" :options="options" :series="myseries"></apexcharts>
  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'DonutChart',
  components: {
    apexcharts: VueApexCharts,
  },
  props: ['myseries'],
  data: function() {
    return {
      options: {
        labels: ['Postivie Corolation', 'Negative Corolation'],
        legend: {
        horizontalAlign: 'left',
        position: 'bottom',
          show: true,
          labels: {
            colors: ['#546E7A', '#E91E63'],
            useSeriesColors: false,
          },
        }
      },

    }
  },
}
</script>
