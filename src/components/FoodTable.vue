<template>
  <div id="food-table">
    <p v-if="foods.length < 1" class="empty-table">
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
        <tr v-for="food in foods" :key="food.date">
          <td v-if="editing === food.date">
            <input type="text" v-model="food.date" />
          </td>
          <td v-else>{{ food.date }}</td>
          <td v-if="editing === food.date">
            <input type="text" v-model="food.meal" />
          </td>
          <td v-else>{{ food.meal }}</td>
          <td v-if="editing === food.date">
            <button @click="editfood(food)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(food.date)">Edit</button>
            <button @click="$emit('delete:food', food.date)">
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
  name: 'food-table',
  props: {
    foods: Array
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

    cancelEdit(food) {
      Object.assign(food, this.cachedfood);
      this.editing = null;
    },
    editfood(food) {
      if (food.name === '' || food.email === '') return;
      this.$emit('edit:food', food.id, food);
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
