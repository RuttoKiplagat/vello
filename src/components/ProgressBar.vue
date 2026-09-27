<script setup>
import { computed } from 'vue';

const props = defineProps({
  current: {
    type: Number,
    required: true
  },
  total: {
    type: Number,
    required: true
  }
});

const progress = computed(() => {
  return (props.current / props.total) * 100;
});
</script>

<template>
  <div class="w-full max-w-md mx-auto mb-8">
    <div class="flex justify-between items-center mb-2 text-sm text-romantic-600 font-medium">
      <span>Question {{ current }} of {{ total }}</span>
      <span>{{ Math.round(progress) }}%</span>
    </div>
    <div class="h-3 w-full bg-romantic-100 rounded-full overflow-hidden shadow-inner">
      <div 
        class="h-full bg-gradient-to-r from-romantic-400 to-romantic-600 transition-all duration-500 ease-out relative"
        :style="{ width: `${progress}%` }"
      >
        <div class="absolute inset-0 bg-white/20 animate-pulse"></div>
      </div>
    </div>
  </div>
</template>
