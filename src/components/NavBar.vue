<template>
  <header class="flex fixed gap-6 w-full z-50 justify-between items-center p-6 bg-white shadow-md">
    <!-- Logo -->
    <img 
      src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" 
      alt="Logo" 
      class="w-48"
    />

    <div class="flex w-full gap-6 justify-end">
      <div class="relative w-full max-w-lg ">
        <input 
  v-model="user_search_restaurant" 
  type="text" 
  placeholder="De quoi avez-vous envie?" 
  class="w-full placeholder:text-black/60 ring-0 focus:ring-0 focus:outline-none h-11 px-4  py-2 rounded-full bg-gray-100 border transition-colors border-gray-200 "
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

      <!-- Navigation Icons -->
      <nav class="flex items-center gap-4">
        <!-- Panier -->
        <router-link 
          to="/cart" 
          class="flex items-center justify-center hover:border-green-400 h-11 px-3 rounded-full bg-gray-100 border border-gray-200 hover:bg-green-400 group transition-colors"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 opacity-80 group-hover:invert" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
          </svg>
        </router-link>



        <!-- Compte -->
        <div class="relative group">
          <button class="flex items-center justify-center hover:border-green-400 h-11 px-3 rounded-full bg-gray-100 border border-gray-200 hover:bg-green-400 transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 opacity-80 group-hover:invert" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
            </svg>
          </button>

          <!-- Dropdown Menu -->
          <div class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg py-1 hidden group-hover:block">
            <router-link to="/profile" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">
              Mon profil
            </router-link>
            <router-link to="/addresses" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">
              Mes adresses
            </router-link>
            <router-link to="/payment-methods" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">
              Moyens de paiement
            </router-link>
            <hr class="my-1">
            <button @click="logout" class="block w-full text-left px-4 py-2 text-sm text-red-600 hover:bg-gray-100">
              Déconnexion
            </button>
          </div>
        </div>
      </nav>
    </div>
  </header>
</template>

<script>
import { ref, watch, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import BDD from '../BDD';

export default {
  name: 'NavBar',
  setup() {
    const router = useRouter();
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

    const logout = () => {
      // Ajoutez ici votre logique de déconnexion
      console.log('Déconnexion...');
      router.push('/login');
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
      search_restaurant,
      logout
    };
  }
}
</script>

<style scoped>
.router-link-active {
  color: #10B981; /* text-green-600 */
}
</style>