
<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const employees = ref([]);
    const page = ref(1);
    const totalPages = ref(1);

    const fetchEmployees = async () => {
      try {
        const response = await axios.get(`https://reqres.in/api/users?page=${page.value}`);
        employees.value = response.data.data;
        totalPages.value = response.data.total_pages;
      } catch (error) {
        console.error('Error fetching employees:', error);
      }
    };

    const nextPage = () => {
      if (page.value < totalPages.value) {
        page.value += 1;
        fetchEmployees();
      }
    };

    const prevPage = () => {
      if (page.value > 1) {
        page.value -= 1;
        fetchEmployees();
      }
    };

    onMounted(fetchEmployees);

    return {
      employees,
      page,
      totalPages,
      nextPage,
      prevPage,
    };
  },
};
</script>

<template>
  <div class="employee-list-container">
    <div class="employee-list">
      <div class="employee" v-for="employee in employees" :key="employee.id">
        <img :src="employee.avatar" class="profile-pic" :alt="`${employee.first_name} ${employee.last_name}`">
        <div class="employee-info">
          <p>{{ employee.first_name }} {{ employee.last_name }}</p>
          <a :href="`mailto:${employee.email}`">{{ employee.email }}</a>
        </div>
      </div>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="page === 1">Föregående</button>
      <span>Sida {{ page }} av {{ totalPages }}</span>
      <button @click="nextPage" :disabled="page === totalPages">Nästa</button>
    </div>
  </div>
</template>

<style scoped>
.employee-list-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background-color: #f9f9f9;
}

.employee-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.employee {
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  max-width: 200px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  background-color: white;
  transition: transform 0.3s, box-shadow 0.3s;
}

.employee:hover {
  transform: translateY(-10px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
}

.profile-pic {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 10px;
}
.employee-info p {
  margin: 0;
  font-weight: bold;
  color: #333;
}

.employee-info a {
  color: #3498db;
  text-decoration: none;
}

.employee-info a:hover {
  text-decoration: underline;
}

.pagination {
  margin-top: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
  color: black;
}

.pagination button {
  padding: 8px 12px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  background-color: #3498db;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

.pagination button:hover {
  background-color: #2980b9;
}

.pagination span {
  font-size: 16px;
}

@media (max-width: 600px) {
  .employee-list {
    flex-direction: column;
    align-items: center;
  }

  .employee {
    width: 80%;
  }
}
.employee-info p {
  margin: 0;
  font-weight: bold;
  color: #333;
}

.employee-info a {
  color: #3498db;
  text-decoration: none;
}

.employee-info a:hover {
  text-decoration: underline;
}

.pagination {
  margin-top: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
  color: black;
}

.pagination button {
  padding: 8px 12px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  background-color: #3498db;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

.pagination button:hover {
  background-color: #2980b9;
}


.pagination span {
  font-size: 16px;
}


</style>
