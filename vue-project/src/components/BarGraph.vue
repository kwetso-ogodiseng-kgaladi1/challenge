<template>
  <div class="bar-graph">
    <h2>Monthly Installs</h2>
    <canvas ref="chartCanvas" width="400" height="200"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

export default {
  data() {
    return {
      installs: []
    };
  },
  mounted() {
    // Fetch data from JSON file
    fetch('/src/assets/barGraph.json')
      .then(response => response.json())
      .then(data => {
        this.installs = data;
        this.drawChart();
      })
      .catch(error => console.error('Error fetching data:', error));
  },
  methods: {
    drawChart() {
      const ctx = this.$refs.chartCanvas.getContext('2d');
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: this.installs.map(install => install.month),
          datasets: [{
            label: 'Monthly Installs',
            data: this.installs.map(install => install.numInstalls),
            backgroundColor: 'rgba(54, 162, 235, 0.6)',
            borderColor: 'rgba(54, 162, 235, 1)',
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    }
  }
};
</script>

<style scoped>
/* Add your component-specific styles here */
.bar-graph {
  font-family: Arial, sans-serif;
}
</style>
