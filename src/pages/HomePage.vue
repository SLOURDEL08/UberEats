<template>
  <div class="home-page min-h-screen bg-gray-50">
    <!-- Header with Search -->
    <header class="flex justify-between items-center p-6 bg-white">
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

    <!-- Banner Section -->
    <div 
      class="banner h-[200px] bg-cover bg-center relative"
      style="background-image: url('https://www.uber-assets.com/image/upload/f_auto,q_auto:eco,c_fill,w_1116,h_744/v1622579402/assets/6d/caa1da-ef21-426e-b3be-170af0054fa9/original/WelcomeToUberEats.jpg');"
    >
      <div class="absolute inset-0 bg-gradient-to-b from-black/50 to-black/20"></div>
    </div>

    <!-- Restaurant Rows -->
    <section class="px-6 py-8">
      <h2 class="text-3xl font-bold mb-6">Nos restaurants</h2>
      <RestaurantRow 
        v-for="(data, i) in data_restaurant" 
        :key="i" 
        :three_restaurant="data" 
      />
    </section>
  </div>
</template>

<script>
import BDD from '../BDD'
import { onMounted, ref, watch } from 'vue'
import RestaurantRow from '../components/RestaurantRow.vue'

export default {
  name: 'HomePage',
  components: { RestaurantRow },
  setup() {
    const data_restaurant = ref([]);
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
      let three_restaurant = [];
      for (const restaurant of BDD) {
        const newRestaurant = new Restaurant(
          restaurant.name,
          restaurant.note,
          restaurant.image,
          restaurant.drive_time
        );
        all_restaurant.push(newRestaurant);
        three_restaurant.push(newRestaurant);

        // Regroupe les restaurants en lots de trois
        if (three_restaurant.length === 3) {
          data_restaurant.value.push([...three_restaurant]);
          three_restaurant = [];
        }
      }

      // S'il reste moins de trois restaurants, ajoutez-les comme un dernier groupe
      if (three_restaurant.length) {
        data_restaurant.value.push([...three_restaurant]);
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
      data_restaurant,
      user_search_restaurant,
      search_restaurant
    };
  }
}
</script>

<style scoped>
.banner {
  background-position: center;
  background-size: cover;
}
</style>
