<!-- components/Filters.vue -->
<template>
  <div class="relative flex items-center justify-end gap-4">
    <!-- Filtres déployés à gauche -->
    <TransitionGroup
      name="filter"
      class="flex items-center gap-2"
      tag="div"
    >
      <button 
        v-for="(category, index) in expandedCategories" 
        :key="category"
        @click="selectCategory(category)"
        class="px-4 py-2 bg-white border  border-gray-200 rounded-full text-sm font-medium whitespace-nowrap transition-all duration-300 hover:bg-green-400 hover:text-white hover:border-green-400"
        :class="{
          'border-green-400 bg-green-400 text-white': selectedCategory === category,
          'filter-enter': !isExiting,
          'filter-exit': isExiting
        }"
        :style="{
          '--index': index,
          'animation-delay': `${index * 100}ms`
        }"
      >
        {{ category }}
      </button>
    </TransitionGroup>

    <!-- Bouton principal -->
    <button 
      @click="toggleFilters"
      class="px-4 py-2 bg-white hover:border-black  group border border-gray-200 hover:bg-black hover:text-green-400 rounded-full text-lg font-[500] whitespace-nowrap transition-all duration-300 flex items-center gap-2"
      :class="{'!bg-black  border-black text-green-400': isExpanded || selectedCategory}"
    >
      <span class="material-icons h-6 w-6 transition-all duration-300 group-hover:text-green-400 text-gray-600" 
            :class="{'text-green-400': isExpanded || selectedCategory}">
        filter_list
      </span>
      {{ 'Filtres' }}
      <span 
        class="material-icons h-6 w-6  transition-all duration-300  group-hover:text-green-400  "
        :class="{'rotate-180': isExpanded, 'text-green-400 ': isExpanded || selectedCategory}"
      >
        expand_more
      </span>
    </button>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  name: 'Filters-',
  emits: ['update:selectedCategory'],
  props: {
    selectedCategory: {
      type: String,
      default: ''
    }
  },
  setup(props, { emit }) {
    const isExpanded = ref(false);
    const isExiting = ref(false);
    
    const categories = [
      'Burger', 'Pizza', 'Japonais', 'Tacos', 
      'Sandwitch', 'Indien', 'Healthy'
    ];

    const expandedCategories = computed(() => {
      return isExpanded.value ? categories : [];
    });

    const toggleFilters = () => {
      if (isExpanded.value) {
        isExiting.value = true;
        setTimeout(() => {
          isExpanded.value = false;
          isExiting.value = false;
        }, categories.length * 100 + 300); // Durée totale des animations + marge
      } else {
        isExiting.value = false;
        isExpanded.value = true;
      }
    };

    const selectCategory = (category) => {
      if (props.selectedCategory === category) {
        emit('update:selectedCategory', '');
      } else {
        emit('update:selectedCategory', category);
      }
    };

    return {
      categories,
      isExpanded,
      isExiting,
      expandedCategories,
      selectCategory,
      toggleFilters
    };
  }
}
</script>

<style scoped>
.filter-enter {
  animation: filterAppear 0.3s ease forwards;
  animation-delay: calc(var(--index) * 100ms);
  opacity: 0;
}

.filter-exit {
  animation: filterDisappear 0.3s ease forwards;
  animation-delay: calc(var(--index) * 100ms);
}

@keyframes filterAppear {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes filterDisappear {
  from {
    opacity: 1;
    transform: translateX(0);
  }
  to {
    opacity: 0;
    transform: translateX(20px);
  }
}

.scrollbar-hide::-webkit-scrollbar {
  display: none;
}

.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>