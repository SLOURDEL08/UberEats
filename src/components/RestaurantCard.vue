<template>
  <div 
    class="restaurant-card group bg-white rounded-3xl overflow-hidden cursor-pointer transform transition-all duration-300 border hover:shadow-2xl hover:-translate-y-1"
  >
    <!-- Image Container -->
    <div class="relative h-60 overflow-hidden">
      <div 
        class="absolute inset-0 bg-center bg-cover transform transition-transform duration-700 group-hover:scale-110" 
        :style="changeBackground"
      >
        <!-- Gradient Overlay -->
        <div class="absolute inset-0 bg-gradient-to-t from-black/50 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
      </div>

      <!-- Status Badge -->
      <div 
        :class="[
          'absolute top-4 left-4 px-3 py-1.5 rounded-full text-xs font-medium transition-transform duration-300 transform group-hover:translate-y-1',
          info_restaurant.is_open ? 'bg-green-500 text-white' : 'bg-red-500 text-white'
        ]"
      >
        {{ info_restaurant.is_open ? 'Ouvert' : 'Fermé' }}
      </div>

      <div class="absolute bottom-0 left-0 right-0 p-4 text-white transform translate-y-full group-hover:translate-y-0 transition-transform duration-300">
        <div class="flex items-center gap-3 text-sm">
          <span class="flex items-center gap-1">
            <span class="material-icons text-base">local_offer</span>
            Livraison gratuite
          </span>
          <span class="flex items-center gap-1">
            <span class="material-icons text-base">verified</span>
            Partenaire certifié
          </span>
        </div>
      </div>
    </div>

    <!-- Content Container -->
    <div class="p-4">
      <!-- Restaurant Info Header -->
      <div class="flex justify-between items-start mb-3">
        <div class="flex-1">
          <h3 class="text-lg font-bold text-gray-900 group-hover:text-green-600 transition-colors duration-300 line-clamp-1">
            {{ info_restaurant.name }}
          </h3>
          <p class="text-sm text-gray-500 line-clamp-1">{{ info_restaurant.cuisine }}</p>
        </div>
        <div class="flex items-center bg-yellow-50 text-yellow-700 px-2.5 py-1 rounded-lg ml-2">
          <span class="material-icons text-sm mr-1">star</span>
          <span class="font-semibold">{{ info_restaurant.note }}</span>
        </div>
      </div>

      <!-- Restaurant Details -->
      <div class="flex items-center gap-4 text-sm text-gray-600">
        <span class="flex items-center gap-1.5">
          <span class="material-icons text-gray-400">schedule</span>
          <span>{{ info_restaurant.drive_time }}</span>
        </span>
        <span class="flex items-center gap-1.5">
          <span class="material-icons text-gray-400">payments</span>
          <span>Frais: 2,50 €</span>
        </span>
      </div>

      <!-- Tags Container -->
      <div class="mt-3 flex flex-wrap gap-2">
        <span 
          v-for="tag in ['Populaire', 'Desserts', 'Fruits de mer']" 
          :key="tag"
          class="px-2 py-1 text-xs font-medium text-gray-600 bg-gray-100 rounded-full hover:bg-gray-200 transition-colors"
        >
          {{ tag }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue'

export default {
  name: "RestaurantCard",
  props: { 
    info_restaurant: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const changeBackground = computed(() => ({
      backgroundImage: `url(${props.info_restaurant.image})`
    }));

    return { changeBackground };
  }
}
</script>

<style scoped>
.line-clamp-1 {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Optional: Add a smooth transition when hovering out */
.restaurant-card {
  will-change: transform;
}
</style>