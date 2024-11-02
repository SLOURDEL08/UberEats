<template>
  <div class="restaurant-row mb-8">
    <div class="flex justify-between gap-8">
      <RouterLink
        v-for="(restaurant, index) in three_restaurant"
        :key="index"
        :to="{ name: 'Restaurant', params: { name: restaurant.name } }"
        class="restaurant-wrapper w-1/3"
      >
        <RestaurantCard :info_restaurant="restaurant" />
      </RouterLink>
    </div>
  </div>
</template>

<script>
import RestaurantCard from './RestaurantCard.vue'

export default {
  name: "RestaurantRow",
  components: { RestaurantCard },
  props: { three_restaurant: Array }
}
</script>

<style scoped>
.restaurant-wrapper {
  transition: transform 0.3s;
}
.restaurant-wrapper:hover {
  transform: translateY(-5px);
}
</style>
