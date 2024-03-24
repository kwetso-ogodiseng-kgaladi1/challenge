<template>
  <div>
    <h1>Monthly Installs</h1>
    <div v-if="graphData.length > 0" class="bar-container">
      <div v-for="(data, index) in graphData" :key="index" class="bar">
        <div class="bar-label">{{ data.month }}</div>
        <div class="bar-fill" :style="{ height: (data.numInstalls * 10) + 'px' }">{{ data.numInstalls }}</div>
      </div>
    </div>
    <div v-else>
      Loading...
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      graphData: []
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch('/src/assets/barGraph.json') // Adjust the path as per your file structure
        .then(response => response.json())
        .then(data => {
          this.graphData = data;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    }
  }
};
</script>

<style>
.bar-container {
  display: flex;
}

.bar {
  margin: 0 10px;
  border: 1px solid #000;
  text-align: center;
  width: 50px;
}

.bar-label {
  font-size: 12px;
}

.bar-fill {
  background-color: #3498db;
  color: #fff;
}
</style>
