<template>
  <!-- Main App Component -->
  <!-- Header with background image -->
    <header class="header">
      
    </header>
  <div id="app" class="container">
    <!-- Bar Graph Component -->
    <BarGraph />
    <!-- Pie Chart Component -->
    <PieChart />
    <!-- Users Component -->
    <Users />
  </div>
</template>

<script>
// Importing components
import BarGraph from './components/BarGraph.vue';
import PieChart from './components/PieChart.vue';
import Users from './components/Users.vue';

export default {
  components: {
    // Registering components
    BarGraph,
    PieChart,
    Users
  }
};
</script>

<style scoped>
/* Component-specific styles */
.container {
  background-color: #f0f0f0; /* Set background color */
  padding: 20px; /* Add padding */
  display: flex; /* Use flexbox layout */
  flex-direction: column; /* Align components vertically */
  align-items: center; /* Center components horizontally */
  justify-content: center; /* Center components vertically */
}

header {
  background-image: url('/header.png'); /* Set background image */
  background-size: cover; /* Cover the entire header */
  background-position: center; /* Center the background image */
  width: 100%; /* Set header width to cover the entire container */
  height: 200px; /* Set header height */
  position: relative; 
}

@media (max-width: 768px) {
  .header {
    height: 150px; /* Decrease height on smaller screens */
  }
}
</style>
