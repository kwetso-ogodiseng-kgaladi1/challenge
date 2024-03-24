<template>
  <div class="bar-graph" ref="chartContainer">
    <h2>Monthly Installs</h2>
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

export default {
  data() {
    return {
      installs: [],
      chart: null
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

    // Listen for resize events
    window.addEventListener('resize', this.resizeChart);
  },
  methods: {
    drawChart() {
      const ctx = this.$refs.chartCanvas.getContext('2d');
      this.chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: this.installs.map(install => install.month),
          datasets: [{
            label: 'Monthly Installs',
            data: this.installs.map(install => install.numInstalls),
            backgroundColor: '#a51890',
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
      this.resizeChart(); // Initial resizing
    },
    resizeChart() {
      const container = this.$refs.chartContainer;
      const canvas = this.$refs.chartCanvas;
      // Set maximum width and height
      const maxWidth = container.clientWidth;
      const maxHeight = container.clientHeight;
      const aspectRatio = canvas.width / canvas.height;
      let newWidth = maxWidth;
      let newHeight = maxWidth / aspectRatio;
      if (newHeight > maxHeight) {
        newHeight = maxHeight;
        newWidth = maxHeight * aspectRatio;
      }
      // Update canvas dimensions
      canvas.width = newWidth;
      canvas.height = newHeight;
      // Update chart size
      this.chart.resize();
    }
  },
  beforeUnmount() {
    // Clean up resize event listener
    window.removeEventListener('resize', this.resizeChart);
  }
};
</script>

<style scoped>
/* Add your component-specific styles here */
.bar-graph {
  font-family: Arial, sans-serif;
  width: 100%; 
  height: 100%; 
}
</style>
