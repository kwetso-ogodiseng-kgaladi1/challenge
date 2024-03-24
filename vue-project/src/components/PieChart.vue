<template>
  <div>
    <h1>Yearly Installs</h1>
    <div v-if="installData.length > 0" class="bar-container">
      <div v-for="(data, index) in installData" :key="index" class="bar">
        <div class="bar-label">{{ data.year }}</div>
        <div class="bar-fill">{{ data.numInstalls }}</div>
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
      installData: []
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch('/src/assets/pieChart.json') 
        .then(response => response.json())
        .then(data => {
          this.installData = data;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    }
  }
};
</script>
