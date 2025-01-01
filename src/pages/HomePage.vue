<template>
  <div class="home-page min-h-screen bg-gray-50">
    <BannerSlider />

    <section class="px-6 py-8">
      <!-- Header with Filters -->
      <div class="flex flex-col gap-6 mb-8">
        <div class="flex items-center justify-between">
          <h2 class="text-3xl font-bold">Nos restaurants</h2>
          <div class="flex items-center gap-4">
            <Filters v-model:selectedCategory="selectedCategory" />
          </div>
        </div>

      
      </div>

      <!-- Restaurant Grid -->
      <div v-if="selectedCategory" class="grid grid-cols-3  gap-6">
        <div 
          v-for="restaurant in filteredRestaurants" 
          :key="restaurant.name"
          class="w-full"
        >
          <RouterLink 
            :to="{ name: 'Restaurant', params: { name: restaurant.name } }"
          >
            <RestaurantCard :info_restaurant="restaurant" />
          </RouterLink>
        </div>
      </div>

      <!-- Default Grid Display -->
      <div v-else class="grid grid-cols-3 max-md:grid-cols-2 max-sm:grid-cols-1 gap-6">
        <div 
          v-for="restaurant in BDD" 
          :key="restaurant.name"
          class="w-full"
        >
          <RouterLink 
            :to="{ name: 'Restaurant', params: { name: restaurant.name } }"
          >
            <RestaurantCard :info_restaurant="restaurant" />
          </RouterLink>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import BDD from '../BDD'
import { ref, computed } from 'vue'
import RestaurantCard from '../components/RestaurantCard.vue'
import BannerSlider from '../components/BannerSlider.vue'
import Filters from '../components/Filters.vue'
import { RouterLink } from 'vue-router'
import { watch } from 'vue'

export default {
  name: 'HomePage',
  components: { 
    RestaurantCard, 
    BannerSlider, 
    Filters,
    RouterLink
  },
  setup() {
    const selectedCategory = ref('');
    const showFilters = ref(false);
    
    const categories = [
      'Burger', 'Pizza', 'Japonais', 'Tacos', 
      'Sandwitch', 'Indien', 'Healthy'
    ];

    // Restaurant categories mapping
    const restaurantCategories = {
      'McDonald\'s': 'Burger',
      'Burger\'s Friens': 'Burger',
      'Pizza Hut': 'Pizza',
      'Momiji': 'Japonais',
      'O\'Tacos': 'Tacos',
      'Subway': 'Sandwitch',
      'L\'escale': 'Indien',
      'Food Time': 'Burger',
      'M&A': 'Healthy'
    };

    const filteredRestaurants = computed(() => {
      if (!selectedCategory.value) return BDD;
      return BDD.filter(restaurant => 
        restaurantCategories[restaurant.name] === selectedCategory.value
      );
    });

    const selectCategory = (category) => {
      selectedCategory.value = selectedCategory.value === category ? '' : category;
    };

    // Watch pour synchroniser showFilters avec Filters component
    watch(() => selectedCategory.value, (newVal) => {
      showFilters.value = !!newVal;
    });

    return {
      BDD,
      selectedCategory,
      categories,
      showFilters,
      filteredRestaurants,
      selectCategory,
      restaurantCategories
    };
  }
}
</script>

<style scoped>
/* Animations pour les filtres */
.filters-enter-active,
.filters-leave-active {
  transition: all 0.3s ease-out;
}

.filters-enter-from,
.filters-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>