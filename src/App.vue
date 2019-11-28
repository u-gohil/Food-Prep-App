<template>
  <div id="app" class="small-container">
    <h1>Meal Prep</h1>
    <food-form @add:employee="addEmployee" />

    <food-table
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import FoodTable from '@/components/FoodTable.vue';
import FoodForm from '@/components/FoodForm.vue';

export default {
  name: 'app',
  components: {
    FoodTable,
    FoodForm
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch(
          'http://my-json-server.typicode.com/756D6D61/MealApp-Data/data'
        );
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        return error;
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch(
          'http://my-json-server.typicode.com/756D6D61/MealApp-Data/data',
          {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }
          }
        );
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        return error;
      }
    },
    deleteEmployee(date) {
      this.employees = this.employees.filter(
        (this.employees = this.employees.filter(
          employee => employee.date !== date
        ))
      );
    },
    async editEmployee(date, updatedEmployee) {
      try {
        const response = await fetch(
          `http://my-json-server.typicode.com/756D6D61/MealApp-Data/data/${date}`,
          {
            method: 'PUT',
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }
          }
        );
        const data = await response.json();
        this.employees = this.employees.map(employee =>
          employee.date === date ? data : employee
        );
      } catch (error) {
        return error;
      }
    }
  },

  data() {
    return {
      employees: []
    };
  },
  mounted() {
    this.getEmployees();
  },

  async deleteEmployee(date) {
    try {
      await fetch(
        `http://my-json-server.typicode.com/756D6D61/MealApp-Data/data/${date}`,
        {
          method: 'DELETE'
        }
      );
      this.employees = this.employees.filter(
        employee => employee.date !== date
      );
    } catch (error) {
      return error;
    }
  }
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
