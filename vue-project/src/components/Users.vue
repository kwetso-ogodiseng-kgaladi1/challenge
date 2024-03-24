<template>
  <div class="employee-list">
    <h2>Employee List</h2>
    <div class="filter">
      <label for="designation">Filter by Designation:</label>
      <select v-model="selectedDesignation" id="designation">
        <option value="">All Designations</option>
        <option v-for="designation in designations" :value="designation">{{ designation }}</option>
      </select>
    </div>
    <input type="text" v-model="searchQuery" placeholder="Search..." class="search-input">
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Surname</th>
            <th>Designation</th>
            <th>Department</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in paginatedEmployees" :key="employee.surname">
            <td>{{ employee.name }}</td>
            <td>{{ employee.surname }}</td>
            <td>{{ employee.designation }}</td>
            <td>{{ employee.department }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1" class="prev-btn">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages" class="next-btn">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      employees: [],
      searchQuery: '',
      selectedDesignation: '',
      designations: [],
      currentPage: 1,
      itemsPerPage: 10
    };
  },
  computed: {
    filteredEmployees() {
      let filtered = this.employees;

      if (this.selectedDesignation) {
        filtered = filtered.filter(employee => employee.designation === this.selectedDesignation);
      }

      const query = this.searchQuery.toLowerCase();
      return filtered.filter(employee =>
        employee.name.toLowerCase().includes(query) ||
        employee.surname.toLowerCase().includes(query) ||
        employee.designation.toLowerCase().includes(query) ||
        employee.department.toLowerCase().includes(query)
      );
    },
    paginatedEmployees() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      return this.filteredEmployees.slice(startIndex, startIndex + this.itemsPerPage);
    },
    totalPages() {
      return Math.ceil(this.filteredEmployees.length / this.itemsPerPage);
    }
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    }
  },
  mounted() {
    // Fetch data from JSON file
    fetch('/src/assets/Users.json')
      .then(response => response.json())
      .then(data => {
        this.employees = data;
        this.designations = [...new Set(data.map(employee => employee.designation))];
      })
      .catch(error => console.error('Error fetching data:', error));
  }
};
</script>

<style scoped>
.employee-list {
  margin: 20px auto;
  max-width: 800px;
  font-family: Arial, sans-serif;
}

.filter {
  margin-bottom: 10px;
}

.filter label {
  font-weight: bold;
  margin-right: 10px;
}

.search-input {
  margin-bottom: 10px;
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  border-radius: 5px;
  overflow: hidden;
}

th, td {
  border: 1px solid #dddddd;
  padding: 12px;
  text-align: left;
}

th {
  background-color: #4CAF50;
  color: white;
  font-weight: bold;
}

tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}

tbody tr:hover {
  background-color: #e7e7e7;
}

.pagination {
  margin-top: 20px;
  text-align: center;
}

.pagination button {
  padding: 8px 16px;
  margin: 0 5px;
  cursor: pointer;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.pagination button:hover {
  background-color: #45a049;
}

.pagination button:disabled {
  cursor: not-allowed;
  background-color: #cccccc;
}

.prev-btn {
  background-color: #f44336;
}

.prev-btn:hover {
  background-color: #d32f2f;
}

.next-btn {
  background-color: #2196F3;
}

.next-btn:hover {
  background-color: #1976D2;
}

.pagination span {
  margin: 0 10px;
}
</style>
