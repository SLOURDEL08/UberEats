<template>
  <div class="relative h-[500px] overflow-hidden group">
    <TransitionGroup name="fade">
      <div
        v-for="(slide, index) in slides"
        :key="slide.title"
        v-show="currentSlide === index"
        class="absolute inset-0"
      >
        <!-- Background Image -->
        <div
          class="absolute inset-0 bg-cover bg-bottom transition-transform duration-[2000ms] ease-in-out"
          :style="{ backgroundImage: `url(${slide.image})`, transform: currentSlide === index ? 'scale(1.1)' : 'scale(1.05)' }"
        />

        <!-- Gradient Overlay -->
        <div class="absolute inset-0 bg-gradient-to-r from-black/80 to-transparent" />

        <!-- Slide Content -->
        <div class="relative pt-16 w-full h-full flex items-center justify-start px-8 md:px-12">
          <div class="max-w-lg">
            <h1 class="text-4xl font-bold text-white mb-4 transition-opacity duration-1000" :class="{ 'opacity-100': currentSlide === index, 'opacity-0': currentSlide !== index }">
              {{ slide.title }}
            </h1>
            <p class="text-lg md:text-xl text-white/90 transition-opacity duration-1000" :class="{ 'opacity-100': currentSlide === index, 'opacity-0': currentSlide !== index }">
              {{ slide.subtitle }}
            </p>
            <button class="mt-6 bg-white text-black px-8 py-3 rounded-full hover:bg-gray-100 transition-transform duration-300 transform hover:scale-105">
              Commander maintenant
            </button>
          </div>
        </div>
      </div>
    </TransitionGroup>

    <!-- Slide Indicators -->
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

    <!-- Navigation Arrows -->

    <button @click="nextSlide" class="absolute right-10 hover:bg-green-400 hover:text-white transition duration-300 top-1/2 backdrop-blur-sm transform -translate-y-1/2 bg-white bg-opacity-50 hover:bg-opacity-80 p-2 rounded-full">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white " fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M9 5l7 7-7 7" />
      </svg>
    </button>
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

const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % slides.length
}



let intervalId = null

const startSlideShow = () => {
  intervalId = setInterval(nextSlide, 5000)
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
