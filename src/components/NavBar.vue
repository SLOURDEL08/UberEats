<!-- components/NavBar.vue -->

<template>
  <header class="flex justify-between items-center p-6 bg-white shadow-md">
    <img 
      src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" 
      alt="Logo" 
      class="w-48"
    />
    <div class="relative w-full max-w-lg">
      <input 
        v-model="user_search_restaurant" 
        type="text" 
        placeholder="De quoi avez-vous envie ?" 
        class="w-full px-4 py-3 rounded-full bg-gray-100 border border-gray-200 focus:ring focus:ring-gray-300"
      />
      <div class="absolute w-full bg-white shadow-lg z-10 mt-1 rounded-lg overflow-hidden" v-if="search_restaurant.length">
        <router-link
          v-for="(restaurant, i) in search_restaurant"
          :key="i"
          :to="{ name: 'Restaurant', params: { name: restaurant.name } }"
          class="block px-4 py-3 hover:bg-gray-100 flex items-center gap-4"
        >
          <img :src="restaurant.image" alt="" class="w-10 h-10 rounded-full object-cover" />
          <span>{{ restaurant.name }}</span>
        </router-link>
      </div>
    </div>
  </header>
</template>

<script>
import { ref, watch, onMounted } from 'vue';
import BDD from '../BDD';

export default {
  name: 'NavBar',
  setup() {
    const all_restaurant = [];
    const user_search_restaurant = ref('');
    const search_restaurant = ref([]);

    class Restaurant {
      constructor(name, note, image, drive_time) {
        this.name = name;
        this.note = note;
        this.image = image;
        this.drive_time = drive_time;
      }
    }

    const makeDataRestaurant = () => {
      for (const restaurant of BDD) {
        const newRestaurant = new Restaurant(
          restaurant.name,
          restaurant.note,
          restaurant.image,
          restaurant.drive_time
        );
        all_restaurant.push(newRestaurant);
      }
    };

    watch(user_search_restaurant, (newValue) => {
      const regex = new RegExp(newValue.toLowerCase());
      search_restaurant.value = newValue
        ? all_restaurant.filter((restaurant) => regex.test(restaurant.name.toLowerCase()))
        : [];
    });

    onMounted(makeDataRestaurant);

    return {
      user_search_restaurant,
      search_restaurant
    };
  }
}
</script>

<style scoped>
/* Ajoutez des styles spécifiques à NavBar si nécessaire */
</style>
