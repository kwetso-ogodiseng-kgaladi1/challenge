<template>
  <div>
    <h2>Employee List</h2>
    <ul>
      <li v-for="employee in employees" :key="employee.surname">
        {{ employee.name }} {{ employee.surname }} - {{ employee.designation }} ({{ employee.department }})
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      employees: []
    };
  },
  mounted() {
    // Fetch data from JSON file
    fetch('/src/assets/Users.json')
      .then(response => response.json())
      .then(data => this.employees = data)
      .catch(error => console.error('Error fetching data:', error));
  }
};
</script>

<style scoped>
/* Add your component-specific styles here */
</style>
