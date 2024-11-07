<template>
  <div class="">
    <div 
      class=" w-full min-h-screen bg-white shadow-xl translate-x-0 transition-transform duration-300 border-l border-gray-200"
   
    >
      <div class="p-4 border-b flex justify-between items-center bg-white">
        <h2 class="text-lg font-bold">Votre commande</h2>
        <button 
          @click="$emit('close')" 
          class="p-2 hover:bg-gray-100 rounded-full lg:hidden"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <div class="h-[calc(100vh-180px)] overflow-y-auto p-4">
        <div v-if="items.length === 0" class="text-center text-gray-500 mt-8">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto mb-4 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
          </svg>
          Votre panier est vide
        </div>
        
        <div v-else class="space-y-4">
          <div v-for="(item, index) in items" :key="index" class="flex items-center gap-4 p-4 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors">
            <img :src="item.image" class="w-20 h-20 object-cover rounded" :alt="item.name"/>
            <div class="flex-1">
              <h3 class="font-medium">{{ item.name }}</h3>
              <p class="text-gray-600 text-sm">{{ item.price }}€</p>
              <div class="flex items-center gap-2 mt-2">
                <button 
                  @click.stop="updateQuantity(index, -1)"
                  class="w-8 h-8 flex items-center justify-center rounded-full border border-gray-300 hover:bg-gray-200"
                >
                  -
                </button>
                <span class="w-8 text-center">{{ item.quantity }}</span>
                <button 
                  @click.stop="updateQuantity(index, 1)"
                  class="w-8 h-8 flex items-center justify-center rounded-full border border-gray-300 hover:bg-gray-200"
                >
                  +
                </button>
              </div>
            </div>
            <button 
              @click.stop="removeItem(index)"
              class="p-2 hover:bg-gray-200 rounded-full"
              title="Supprimer"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>
          </div>
        </div>
      </div>

      <div class="absolute bottom-0 left-0 right-0 p-4 border-t bg-white">
        <div class="space-y-2 mb-4">
          <div class="flex justify-between text-sm">
            <span>Sous-total</span>
            <span>{{ subtotal.toFixed(2) }}€</span>
          </div>
          <div class="flex justify-between text-sm">
            <span>Frais de livraison</span>
            <span>{{ deliveryFee.toFixed(2) }}€</span>
          </div>
          <div class="flex justify-between font-bold pt-2 border-t">
            <span>Total</span>
            <span>{{ total.toFixed(2) }}€</span>
          </div>
        </div>
        <button 
          @click="checkout"
          class="w-full bg-black text-white py-3 rounded-lg font-medium hover:bg-gray-900 transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
          :disabled="items.length === 0"
        >
          {{ items.length === 0 ? 'Panier vide' : 'Commander maintenant' }}
        </button>
      </div>
    </div>

   
  </div>
</template>

<script>
import { computed } from 'vue'

export default {
  name: 'CartSidebar',
  props: {
    isOpen: {
      type: Boolean,
      default: true 
    },
    items: {
      type: Array,
      default: () => []
    }
  },
  emits: ['close', 'update-quantity', 'remove-item', 'checkout'],
  setup(props, { emit }) {
    const deliveryFee = 2.99

    const subtotal = computed(() => {
      return props.items.reduce((total, item) => {
        return total + (item.price * item.quantity)
      }, 0)
    })

    const total = computed(() => {
      return subtotal.value + deliveryFee
    })

    const updateQuantity = (index, change) => {
      emit('update-quantity', { index, change })
    }

    const removeItem = (index) => {
      emit('remove-item', index)
    }

    const checkout = () => {
      if (props.items.length > 0) {
        emit('checkout')
      }
    }

    return {
      subtotal,
      total,
      deliveryFee,
      updateQuantity,
      removeItem,
      checkout
    }
  }
}
</script>

<style scoped>
.transform {
  will-change: transform;
}
</style>