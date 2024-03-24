<script>
    new Vue({
      el: '#app',
      data: {
        users: [],
        selectedDesignation: "",
        searchQuery: "",
        itemsPerPage: 10,
        currentPage: 1
      },
      computed: {
        filteredUsers() {
          let filtered = this.users;

          if (this.selectedDesignation) {
            filtered = filtered.filter(user => user.designation === this.selectedDesignation);
          }

          if (this.searchQuery) {
            const query = this.searchQuery.toLowerCase();
            filtered = filtered.filter(user => 
              user.name.toLowerCase().includes(query) ||
              user.surname.toLowerCase().includes(query) ||
              user.designation.toLowerCase().includes(query) ||
              user.department.toLowerCase().includes(query)
            );
          }

          const startIndex = (this.currentPage - 1) * this.itemsPerPage;
          const endIndex = startIndex + this.itemsPerPage;
          return filtered.slice(startIndex, endIndex);
        },
        totalPages() {
          return Math.ceil(this.filteredUsers.length / this.itemsPerPage);
        },
        designations() {
          return [...new Set(this.users.map(user => user.designation))];
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
        axios.get('users.json')
          .then(response => {
            this.users = response.data;
          })
          .catch(error => {
            console.error('Error fetching users data:', error);
          });
      }
    });
  </script>