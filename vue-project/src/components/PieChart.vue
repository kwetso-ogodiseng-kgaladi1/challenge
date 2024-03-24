<template>
  <div>
    <h2>Yearly Installs</h2>
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
    fetch('/src/assets/pieChart.json')
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
        type: 'pie',
        data: {
          labels: this.installs.map(install => install.year),
          datasets: [{
            label: 'Yearly Installs',
            data: this.installs.map(install => install.numInstalls),
            backgroundColor: [
              'rgba(255, 99, 132, 0.6)',
              'rgba(54, 162, 235, 0.6)',
              'rgba(255, 206, 86, 0.6)',
              'rgba(75, 192, 192, 0.6)',
              'rgba(153, 102, 255, 0.6)',
              'rgba(255, 159, 64, 0.6)'
            ],
            borderColor: [
              'rgba(255, 99, 132, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)'
            ],
            borderWidth: 1
          }]
        }
      });
    }
  }
};
</script>

<style scoped>
/* Add your component-specific styles here */
</style>
