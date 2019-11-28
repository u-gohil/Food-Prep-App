<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Date</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.date"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Ingredients/Meal</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.meal"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out all fields
      </p>
      <p v-if="success" class="success-message">
        ✅ Meal successfully added
      </p>
      <button>Add Meal</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'employee-form',
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        date: '',
        meal: ''
      }
    };
  },
  computed: {
    invalidDate() {
      return this.employee.date === '';
    }
  },
  invalidMeal() {
    return this.employee.meal === '';
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidDate || this.invalidMeal) {
        this.error = true;
        return;
      }

      this.$emit('add:employee', this.employee);
      this.$refs.first.focus();
      this.employee = {
        date: '',
        meal: ''
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*='-message'] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
