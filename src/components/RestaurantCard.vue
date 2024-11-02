<template>
  <div class="restaurant-card p-4 bg-white border rounded-xl hover:scale-105">
    <!-- Image du restaurant avec une étiquette d'ouverture -->
    <div class="relative h-40 bg-center bg-cover rounded-lg mb-4" :style="changeBackground">
      <div 
        v-if="info_restaurant.is_open" 
        class="absolute top-2 left-2 bg-green-500 text-white text-xs font-bold px-2 py-1 rounded-full"
      >
        Ouvert
      </div>
      <div 
        v-else 
        class="absolute top-2 left-2 bg-red-500 text-white text-xs font-bold px-2 py-1 rounded-full"
      >
        Fermé
      </div>
    </div>

    <!-- Nom et note du restaurant -->
    <div class="flex justify-between items-center mb-2">
      <h3 class="text-lg font-semibold truncate hover:underline">{{ info_restaurant.name }}</h3>
      <div class="flex items-center bg-yellow-100 text-yellow-600 text-xs font-bold px-2 py-1 rounded-full">
        <span class="material-icons text-sm mr-1">star</span> {{ info_restaurant.note }}
      </div>
    </div>

    <!-- Informations supplémentaires: temps de livraison, type de cuisine -->
    <div class="text-sm text-gray-500 flex items-center gap-2">
      <span class="flex items-center gap-1">
        <span class="material-icons text-base">schedule</span> 
        {{ info_restaurant.drive_time }}
      </span>
      <span class="flex items-center gap-1">
        <span class="material-icons text-base">restaurant_menu</span>
        {{ info_restaurant.cuisine }}
      </span>
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
.restaurant-card {
  transition: transform 0.3s, box-shadow 0.3s;
}
.restaurant-card:hover {
  box-shadow: 0px 6px 14px rgba(0, 0, 0, 0.2);
}
.material-icons {
  font-size: 18px;
}
</style>
