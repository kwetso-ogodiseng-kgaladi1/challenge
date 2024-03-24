<template>
  <div>
    <h2>Yearly Installs</h2>
    <ul>
      <li v-for="install in installs" :key="install.year">
        {{ install.year }}: {{ install.numInstalls }}
      </li>
    </ul>
  </div>
</template>

<script>
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
      .then(data => this.installs = data)
      .catch(error => console.error('Error fetching data:', error));
  }
};
</script>

<style scoped>
/* Add your component-specific styles here */
</style>
