<script setup>
import { ref } from 'vue';
import { Heart, RotateCcw } from 'lucide-vue-next';

const props = defineProps({
  selectedDate: {
    type: String,
    required: true
  }
});

const emit = defineEmits(['restart']);
const isOpen = ref(false);

const openReveal = () => {
  isOpen.value = true;
};
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen p-6 text-center z-10 relative">
    
    <Transition name="fade" mode="out-in">
      <div v-if="!isOpen" class="max-w-md w-full">
        <h2 class="text-3xl md:text-4xl font-bold mb-10 text-romantic-900">
          One last thing...
        </h2>
        
        <button 
          @click="openReveal"
          class="glass-button px-10 py-5 text-2xl w-full shadow-lg shadow-romantic-400/30 animate-pulse"
        >
          Open it ❤️
        </button>
      </div>

      <div v-else class="max-w-2xl w-full">
        <div class="glass-card p-10 md:p-14 relative overflow-hidden bg-white/90">
          <!-- Decorative hearts -->
          <Heart class="absolute top-4 left-4 w-8 h-8 text-romantic-300 opacity-50 -rotate-12" />
          <Heart class="absolute bottom-4 right-4 w-10 h-10 text-romantic-400 opacity-50 rotate-12" />
          <Heart class="absolute top-1/4 right-8 w-6 h-6 text-romantic-200 opacity-50 rotate-45" />
          
          <h1 class="text-5xl md:text-7xl font-bold mb-8 text-transparent bg-clip-text bg-gradient-to-r from-romantic-600 via-purple-600 to-romantic-600 animate-pulse">
            I LOVE YOU,<br/>VELLYN ❤️
          </h1>
          
          <div class="space-y-4 text-xl text-slate-700 font-medium mb-12">
            <p>And I can't wait for our picnic date.</p>
            <p class="text-2xl text-romantic-800 font-bold bg-romantic-50 inline-block px-6 py-3 rounded-full border border-romantic-200">
              Our date: {{ selectedDate }} 🌸
            </p>
            <p class="pt-4 italic text-slate-600">Until then... keep smiling for me. ❤️</p>
          </div>
          
          <button 
            @click="emit('restart')"
            class="flex items-center justify-center gap-2 mx-auto text-slate-500 hover:text-romantic-600 transition-colors"
          >
            <RotateCcw class="w-4 h-4" />
            <span class="text-sm font-medium uppercase tracking-wider">Start Again ❤️</span>
          </button>
        </div>
      </div>
    </Transition>
    
  </div>
</template>
