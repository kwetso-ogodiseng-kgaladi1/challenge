<template>
  <!-- Bar Graph Component -->
  <div class="bar-graph" ref="chartContainer">
    <!-- Title -->
    <h2>Monthly Installs</h2>
    <!-- Canvas for Chart -->
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

export default {
  data() {
    return {
      installs: [], // Array to store install data
      chart: null // Variable to hold chart instance
    };
  },
  mounted() {
    // Fetch data from JSON file
    fetch('/src/assets/barGraph.json')
      .then(response => response.json())
      .then(data => {
        // Store fetched data in 'installs' array
        this.installs = data;
        // Draw chart once data is fetched
        this.drawChart();
      })
      .catch(error => console.error('Error fetching data:', error));

    // Listen for resize events to adjust chart size
    window.addEventListener('resize', this.resizeChart);
  },
  methods: {
    // Method to draw chart using Chart.js
    drawChart() {
      // Get canvas context for drawing
      const ctx = this.$refs.chartCanvas.getContext('2d');
      // Create new chart instance
      this.chart = new Chart(ctx, {
        type: 'bar',
        data: {
          // Set labels and data for the chart
          labels: this.installs.map(install => install.month),
          datasets: [{
            label: 'Monthly Installs',
            data: this.installs.map(install => install.numInstalls),
            backgroundColor: '#a51890', // Bar color
            borderColor: 'rgba(54, 162, 235, 1)', // Border color
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true // Start y-axis from zero
            }
          }
        }
      });
      this.resizeChart(); // Call method to resize chart
    },
    // Method to resize chart based on container size
    resizeChart() {
      const container = this.$refs.chartContainer;
      const canvas = this.$refs.chartCanvas;
      // Set maximum width and height for the chart
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
    // Remove resize event listener before component unmounts
    window.removeEventListener('resize', this.resizeChart);
  }
};
</script>

<style scoped>
/* Component-specific styles */
.bar-graph {
  font-family: Arial, sans-serif;
  width: 100%; 
  height: 100%; 
}
</style>
