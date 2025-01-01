<template>
  <div v-if="restaurant" class="min-h-screen flex w-full bg-gray-50">
    <div class="w-full max-h-screen overflow-y-scroll scrollbar-hide">
      <!-- Hero Section with Image -->
      <div class="relative h-[50vh]">
        <div class="absolute inset-0">
          <img :src="restaurant.image" class="w-full h-full object-cover" alt=""/>
          <div class="absolute inset-0 flex flex-col justify-end bg-gradient-to-t from-black to-black/0">
               <div class="text-white p-10">
            <div class="flex justify-between items-start">
              <div>
                <h1 class="text-3xl font-bold">{{ restaurant.name }}</h1>
                <div class="flex items-center gap-3 mt-3 text-white">
                  <div class="flex items-center gap-1">
                    <svg class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
                      <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/>
                    </svg>
                    <span class="font-medium">{{ restaurant.note }}</span>
                  </div>
                  <span>•</span>
                  <span>{{ restaurant.drive_time }}</span>
                  <span>•</span>
                  <span class="px-2 py-1 bg-gray-100 text-black rounded-full text-sm">€€</span>
                </div>
              </div>
              <div class="flex gap-2">
                <button class="p-2 rounded-full hover:bg-gray-100 hover:text-black">
                  <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
                  </svg>
                </button>
                <button class="p-2 rounded-full hover:bg-gray-100 hover:text-black">
                  <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.367 2.684 3 3 0 00-5.367-2.684z"/>
                  </svg>
                </button>
              </div>
            </div>
          </div>

          </div>
        </div>
        <!-- Back Button -->
        <RouterLink to="/" class="absolute top-32 left-10 z-10">
          <button class="flex items-center gap-2 px-4 py-2 bg-white rounded-full shadow-lg hover:bg-gray-50 transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
            </svg>
            <span class="font-medium text-sm">Retour</span>
          </button>
        </RouterLink>
      </div>

 

      <!-- Menu Section -->
      <div class="py-8 px-4 sm:px-6 lg:px-8">
        <div class="space-y-12">
          <div 
            v-for="category in restaurant.menu" 
            :key="category.category" 
            class="scroll-mt-24"
          >
            <!-- Category Header -->
            <div class="flex items-center justify-between mb-6">
              <div class="flex items-center gap-3">
                <h2 class="text-2xl font-bold">{{ category.category }}</h2>
                <span class="text-sm text-gray-500">{{ category.items.length }} plats</span>
              </div>
              <div class="h-[1px] flex-grow mx-4 bg-gray-100"></div>
            </div>

            <!-- Items Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div
                v-for="item in category.items"
                :key="item.name"
                @click="addToCart(item)"
                class="group relative flex h-40 justify-between  gap-4 bg-white  border border-gray-200 hover:border-black hover:shadow-lg transition-all duration-300 cursor-pointer"
              >
                <!-- Item Content -->
                <div class="flex w-2/3 flex-col gap-4 justify-between items-stretch p-4">
                  <div class="flex justify-between items-start">
                    <h3 class="font-medium text-lg font-semibold text-gray-900  group-hover:text-black transition-colors line-clamp-1">
                      {{ item.name }}
                    </h3>
                    <p class=" p-1.5 px-3 text-sm rounded-full bg-gray-200/50">{{ item.price.toFixed(2) }}€</p>
                  </div>
                  <p class="text-gray-500 text-sm line-clamp-2">
                    {{ item.description || 'Une délicieuse description du plat irait ici' }}
                  </p>
                  <button 
                    class=" group-hover:bg-black w-max group-hover:text-white px-6 py-2 text-sm font-medium text-gray-600 hover:text-white border  hover:bg-green-600 rounded-full transition-colors duration-300"
                  >
                    Ajouter au panier
                  </button>
                </div>

                <!-- Item Image -->
                <div class="w-1/3 h-full">
                  <img
                    :src="item.image"
                    class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
                    :alt="item.name"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
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
    
    const restaurant = ref(null)
    const isCartOpen = ref(true)
    const cartItems = ref([])

    const cartItemCount = computed(() => {
      return cartItems.value.reduce((total, item) => total + item.quantity, 0)
    })

    onMounted(() => {
      const foundRestaurant = BDD.find((r) => r.name === route.params.name)
      if (foundRestaurant) {
        restaurant.value = foundRestaurant
      } else {
        router.push('/')
      }
    })

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
      cartItemCount,
      addToCart,
      updateCartQuantity,
      removeCartItem,
      handleCheckout
    }
  }
}
</script>

<style scoped>
.min-h-screen {
  overflow-x: hidden;
}

.line-clamp-1 {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.scrollbar-hide::-webkit-scrollbar {
  display: none;
}
</style>
