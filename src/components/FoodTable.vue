<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">
      No Meals
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Date</th>
          <th>Ingredients/Meal</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.date">
          <td v-if="editing === employee.date">
            <input type="text" v-model="employee.date" />
          </td>
          <td v-else>{{ employee.date }}</td>
          <td v-if="editing === employee.date">
            <input type="text" v-model="employee.meal" />
          </td>
          <td v-else>{{ employee.meal }}</td>
          <td v-if="editing === employee.date">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(employee.date)">Edit</button>
            <button @click="$emit('delete:employee', employee.date)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'employee-table',
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(id) {
      this.editing = id;
    },

    cancelEdit(employee) {
      Object.assign(employee, this.cachedEmployee);
      this.editing = null;
    },
    editEmployee(employee) {
      if (employee.name === '' || employee.email === '') return;
      this.$emit('edit:employee', employee.id, employee);
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>
