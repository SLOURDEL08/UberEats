<template>
  <div v-if="restaurant" class="min-h-screen flex w-full bg-gray-50"> <!-- Ajout de la marge pour la sidebar -->

    <div class="w-full max-h-screen overflow-y-scroll scrollbar-hidden">
    <!-- Hero Section with Image -->
    <div class="relative h-[45vh]">
      <div class="absolute inset-0">
        <img :src="restaurant.image" class="w-full h-full object-cover" alt=""/>
        <div class="absolute inset-0 bg-gradient-to-b from-black/50 to-black/20"></div>
      </div>
      <!-- Back Button -->
      <RouterLink to="/" class="absolute top-4 left-4 z-10">
        <button class="flex items-center gap-2 px-4 py-2 bg-white rounded-full shadow-lg hover:bg-gray-50 transition-colors">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
          </svg>
          <span class="font-medium text-sm">Retour</span>
        </button>
      </RouterLink>
    </div>

    <!-- Restaurant Info Card -->
    <div class="relative -mt-20 bg-white rounded-t-3xl">
      <div class="mx-auto px-4 sm:px-6 lg:px-8">
        <!-- Restaurant Header -->
        <div class="pt-8 pb-6 border-b">
          <div class="flex justify-between items-start">
            <div>
              <h1 class="text-3xl font-bold">{{ restaurant.name }}</h1>
              <div class="flex items-center gap-3 mt-3 text-gray-600">
                <div class="flex items-center gap-1">
                  <svg class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
                    <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/>
                  </svg>
                  <span class="font-medium">{{ restaurant.note }}</span>
                </div>
                <span>•</span>
                <span>{{ restaurant.drive_time }}</span>
                <span>•</span>
                <span class="px-2 py-1 bg-gray-100 rounded-full text-sm">€€</span>
              </div>
            </div>
            <div class="flex gap-2">
              <button class="p-2 rounded-full hover:bg-gray-100">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
                </svg>
              </button>
              <button class="p-2 rounded-full hover:bg-gray-100">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.367 2.684 3 3 0 00-5.367-2.684z"/>
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Menu Section -->
    <div class="py-8 px-4 sm:px-6 lg:px-8">
      <nav class="flex gap-2 overflow-x-auto pb-4 mb-8 -mx-4 px-4">
        <button
          v-for="category in restaurant.menu"
          :key="category.category"
          @click="setSelectedCategory(category.category)"
          :class="[
            'px-4 py-2 rounded-full whitespace-nowrap font-medium',
            selectedCategory === category.category 
              ? 'bg-black text-white' 
              : 'bg-gray-100 hover:bg-gray-200'
          ]"
        >
          {{ category.category }}
        </button>
      </nav>

      <div v-for="category in restaurant.menu" :key="category.category" class="mb-12">
        <h2 class="text-2xl font-bold mb-6">{{ category.category }}</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div
            v-for="item in category.items"
            :key="item.name"
            @click="addToCart(item)"
            class="flex gap-4 p-4 bg-white rounded-lg border border-gray-100 hover:shadow-md transition-shadow cursor-pointer"
          >
            <div class="flex-1 min-w-0">
              <h3 class="font-medium text-lg truncate">{{ item.name }}</h3>
              <p class="text-gray-600 mt-1">{{ item.description || 'Une délicieuse description du plat irait ici' }}</p>
              <p class="mt-2 font-medium">{{ item.price }}€</p>
            </div>
            <div class="w-24 h-24 flex-shrink-0">
              <img
                :src="item.image"
                class="w-full h-full object-cover rounded-lg"
                alt=""
              />
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Fixed Bottom Bar - Ajusté pour la sidebar -->
</div>

    <!-- Cart Sidebar -->
    <CartSidebar 
      :is-open="true"
      :items="cartItems"
      @close="isCartOpen = false"
      @update-quantity="updateCartQuantity"
      @remove-item="removeCartItem"
      @checkout="handleCheckout"
      class="w-80"
    />
  </div>
  
  <div v-else class="min-h-screen flex items-center justify-center">
    <div class="text-center">
      <div class="text-xl text-gray-500">Chargement...</div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import BDD from '../BDD.js'
import CartSidebar from '../components/CartSidebar.vue'

export default {
  name: 'RestaurantPage',
  components: {
    CartSidebar
  },
  setup() {
    const route = useRoute()
    const router = useRouter()
    
    // Initialisation des refs
    const restaurant = ref(null)
    const isCartOpen = ref(true) // Changé à true par défaut
    const cartItems = ref([])
    const selectedCategory = ref('')

    // Computed properties
    const cartItemCount = computed(() => {
      return cartItems.value.reduce((total, item) => total + item.quantity, 0)
    })

    // Chargement initial des données
    onMounted(() => {
      const foundRestaurant = BDD.find((r) => r.name === route.params.name)
      if (foundRestaurant) {
        restaurant.value = foundRestaurant
        if (foundRestaurant.menu && foundRestaurant.menu.length > 0) {
          selectedCategory.value = foundRestaurant.menu[0].category
        }
      } else {
        router.push('/')
      }
    })

    // Méthodes
    const setSelectedCategory = (category) => {
      selectedCategory.value = category
    }

    const addToCart = (item) => {
      const existingItem = cartItems.value.find(i => i.name === item.name)
      if (existingItem) {
        existingItem.quantity++
      } else {
        cartItems.value.push({
          ...item,
          quantity: 1
        })
      }
      isCartOpen.value = true
    }

    const updateCartQuantity = ({ index, change }) => {
      const item = cartItems.value[index]
      if (item) {
        const newQuantity = item.quantity + change
        if (newQuantity > 0) {
          item.quantity = newQuantity
        } else {
          cartItems.value.splice(index, 1)
        }
      }
    }

    const removeCartItem = (index) => {
      cartItems.value.splice(index, 1)
    }

    const handleCheckout = () => {
      console.log('Procéder au paiement', cartItems.value)
    }

    return {
      restaurant,
      isCartOpen,
      cartItems,
      selectedCategory,
      cartItemCount,
      setSelectedCategory,
      addToCart,
      updateCartQuantity,
      removeCartItem,
      handleCheckout
    }
  }
}
</script>

<style scoped>
/* Pour éviter le scroll horizontal */
.min-h-screen {
  overflow-x: hidden;
}
</style>







