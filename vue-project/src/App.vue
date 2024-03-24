<template>
  <div id="app" class="container">
    <BarGraph />
    <PieChart />
    <Users />
  </div>
</template>

<script>
import BarGraph from './components/BarGraph.vue';
import PieChart from './components/PieChart.vue';
import Users from './components/Users.vue';

export default {
  components: {
    BarGraph,
    PieChart,
    Users
  }
};
</script>

<style scoped>
.container {
  background-color: #f0f0f0;
  padding: 20px;
  display: flex;
  flex-direction: column; /* Align components vertically */
  align-items: center; /* Center components horizontally */
  justify-content: center; /* Center components vertically */
}
</style>
