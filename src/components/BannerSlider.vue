<template>
  <div class="relative h-[500px]  overflow-hidden group">
    <TransitionGroup name="fade">
      <div
        v-for="(slide, index) in slides"
        :key="slide.title"
        v-show="currentSlide === index"
        class="absolute inset-0"
      >
        <div
          class="absolute inset-0 bg-cover bg-bottom transform scale-105 transition-transform duration-[2000ms]"
          :style="{ backgroundImage: `url(${slide.image})` }"
        />
        
        <div class="absolute  inset-0 bg-gradient-to-r from-black/90 to-transparent" />
        
        <div class="relative pt-16 w-full h-full flex items-center  justify-center px-12">
            <div class="w-2/3">
                 <h1 class="text-4xl font-bold text-white mb-4 transform transition-all duration-1000">
            {{ slide.title }}
          </h1>
          <p class="text-xl text-white/90 transform transition-all duration-1000">
            {{ slide.subtitle }}
          </p>
          <button class="mt-8 bg-white text-black px-8 py-3 rounded-full w-fit 
            hover:bg-gray-100 transition-all duration-300 transform hover:scale-105">
            Commander maintenant
          </button>
            </div>
            <div class="w-1/3 grid-cols-3 gap-8">
                <div></div>
                <div></div>
                <div></div>
                <div></div>
                <div></div>
                <div></div>
            </div>
         
        </div>
      </div>
    </TransitionGroup>
    
    <div class="absolute bottom-6 left-1/2 transform -translate-x-1/2 flex gap-2">
      <button
        v-for="(_, index) in slides"
        :key="index"
        @click="setCurrentSlide(index)"
        :class="[
          'w-2 h-2 rounded-full transition-all duration-300',
          index === currentSlide ? 'bg-white w-8' : 'bg-white/50'
        ]"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import Slider1 from '../assets/slide1.jpg'
import Slider2 from '../assets/slide2.png'
import Slider3 from '../assets/slide3.webp'

const currentSlide = ref(0)
const slides = [
  {
    image: Slider1,
    title: 'Vos restaurants préférés livrés chez vous',
    subtitle: 'Commandez maintenant et profitez de la livraison rapide'
  },
  {
    image: Slider2,
    title: 'Des plats de qualité',
    subtitle: 'Découvrez les meilleurs restaurants de votre quartier'
  },
  {
    image: Slider3,
    title: 'Livraison rapide garantie',
    subtitle: 'Vos commandes livrées en moins de 30 minutes'
  }
]

const setCurrentSlide = (index) => {
  currentSlide.value = index
}

let intervalId = null

const startSlideShow = () => {
  intervalId = setInterval(() => {
    currentSlide.value = (currentSlide.value + 1) % slides.length
  }, 5000)
}

onMounted(() => {
  startSlideShow()
})

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId)
})
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}
</style>