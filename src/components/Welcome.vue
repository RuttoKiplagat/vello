<script setup>
import { Heart } from 'lucide-vue-next';

const emit = defineEmits(['start']);
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen p-6 text-center z-10 relative">
    <div class="glass-card p-8 md:p-12 max-w-lg w-full transform transition-all duration-1000 animate-fade-in">
      <Heart class="w-12 h-12 text-romantic-500 mx-auto mb-6 animate-pulse" />
      
      <h1 class="text-4xl md:text-5xl font-bold mb-4 text-glow text-romantic-900">
        Hey Vellyn ❤️
      </h1>
      
      <h2 class="text-xl md:text-2xl text-romantic-700 mb-8 font-medium">
        I made something little for you...
      </h2>
      
      <div class="text-slate-600 space-y-4 mb-10 text-lg leading-relaxed font-sans">
        <p>No pressure. No wrong answers.</p>
        <p>Just you, me, a few questions,<br/>and maybe a little surprise at the end 👀❤️</p>
      </div>
      
      <button 
        @click="emit('start')"
        class="glass-button px-8 py-4 text-xl w-full flex items-center justify-center gap-2 group"
      >
        <span>Let's begin 💕</span>
        <Heart class="w-5 h-5 group-hover:scale-125 transition-transform" />
      </button>
    </div>
  </div>
</template>

<style scoped>
.animate-fade-in {
  animation: fadeIn 1s ease-out forwards;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
