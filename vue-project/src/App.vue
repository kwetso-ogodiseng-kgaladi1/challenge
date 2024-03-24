<template>
  <div id="app">
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

