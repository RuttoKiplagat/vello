<script setup>
import { ref, watch } from 'vue';
import { Heart } from 'lucide-vue-next';

const props = defineProps({
  question: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['answer']);
const selectedIndex = ref(null);
const isAnimating = ref(false);

watch(() => props.question, () => {
  selectedIndex.value = null;
  isAnimating.value = false;
});

const selectAnswer = (index, option) => {
  if (selectedIndex.value !== null) return;
  
  selectedIndex.value = index;
  isAnimating.value = true;
  
  setTimeout(() => {
    emit('answer', option);
  }, 700);
};
</script>

<template>
  <div class="w-full max-w-lg mx-auto z-10 relative">
    <div class="glass-card p-6 md:p-8">
      <h2 class="text-2xl md:text-3xl font-bold mb-8 text-center text-romantic-900 leading-tight">
        {{ question.question }}
      </h2>
      
      <div class="space-y-4">
        <button
          v-for="(option, index) in question.options"
          :key="index"
          @click="selectAnswer(index, option)"
          class="w-full p-4 rounded-xl border text-left transition-all duration-300 relative overflow-hidden group"
          :class="[
            selectedIndex === index 
              ? 'bg-romantic-100 border-romantic-500 shadow-[0_0_15px_rgba(236,72,153,0.3)] scale-[1.02]' 
              : selectedIndex !== null 
                ? 'bg-white/40 border-white/40 opacity-50' 
                : 'bg-white/60 border-white/60 hover:bg-white/90 hover:border-romantic-300 hover:shadow-md'
          ]"
          :disabled="selectedIndex !== null"
        >
          <div class="flex justify-between items-center relative z-10">
            <span class="text-lg font-medium text-slate-700 group-hover:text-romantic-900 transition-colors">
              {{ option }}
            </span>
            <Heart 
              v-if="selectedIndex === index" 
              class="w-6 h-6 text-romantic-500 animate-bounce" 
            />
          </div>
          
          <!-- Ripple effect background -->
          <div 
            v-if="selectedIndex === index" 
            class="absolute inset-0 bg-romantic-200/30 animate-pulse"
          ></div>
        </button>
      </div>
    </div>
  </div>
</template>
