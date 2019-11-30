<template>
  <div id="app" class="small-container">
    <h1>Meal Prep</h1>
    <food-form @add:food="addFood" />

    <food-table
      :foods="foods"
      @delete:food="deleteFood"
      @edit:food="editFood"
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
    async getFood() {
      try {
        const response = await fetch(
          'http://my-json-server.typicode.com/756D6D61/MealApp-Data/data'
        );
        const data = await response.json();
        this.foods = data;
      } catch (error) {
        return error;
      }
    },
    async addFood(food) {
      try {
        const response = await fetch(
          'http://my-json-server.typicode.com/756D6D61/MealApp-Data/data',
          {
            method: 'POST',
            body: JSON.stringify(food),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }
          }
        );
        const data = await response.json();
        this.foods = [...this.foods, data];
      } catch (error) {
        return error;
      }
    },
    deleteFood(date) {
      this.foods = this.foods.filter(
        (this.foods = this.foods.filter(food => food.date !== date))
      );
    },
    async editFood(date, updatedfood) {
      try {
        const response = await fetch(
          `http://my-json-server.typicode.com/756D6D61/MealApp-Data/data/${date}`,
          {
            method: 'PUT',
            body: JSON.stringify(updatedfood),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }
          }
        );
        const data = await response.json();
        this.foods = this.foods.map(food => (food.date === date ? data : food));
      } catch (error) {
        return error;
      }
    }
  },

  data() {
    return {
      foods: []
    };
  },
  mounted() {
    this.getFood();
  },

  async deletefood(date) {
    try {
      await fetch(
        `http://my-json-server.typicode.com/756D6D61/MealApp-Data/data/${date}`,
        {
          method: 'DELETE'
        }
      );
      this.foods = this.foods.filter(food => food.date !== date);
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
