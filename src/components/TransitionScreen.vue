<script setup>
import { ref, onMounted } from 'vue';

const emit = defineEmits(['next']);
const showSecondText = ref(false);
const showButton = ref(false);
const revealQuestion = ref(false);

onMounted(() => {
  setTimeout(() => {
    showSecondText.value = true;
  }, 2000);
  
  setTimeout(() => {
    showButton.value = true;
  }, 4000);
});

const handleReveal = () => {
  revealQuestion.value = true;
};
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen p-6 text-center z-10 relative">
    <div class="max-w-xl w-full">
      
      <Transition name="fade">
        <h2 v-if="!revealQuestion" class="text-3xl md:text-4xl font-bold mb-8 text-romantic-900 leading-tight">
          Okay... enough questions.
        </h2>
      </Transition>

      <Transition name="slide-up">
        <p v-if="showSecondText && !revealQuestion" class="text-xl md:text-2xl text-slate-700 mb-12">
          There's actually one thing I really wanted to ask you... 👀
        </p>
      </Transition>

      <Transition name="fade">
        <button 
          v-if="showButton && !revealQuestion"
          @click="handleReveal"
          class="glass-button px-8 py-4 text-xl mx-auto"
        >
          What is it? ❤️
        </button>
      </Transition>

      <!-- The Big Reveal -->
      <Transition name="slide-up">
        <div v-if="revealQuestion" class="glass-card p-10 mt-8">
          <h1 class="text-4xl md:text-5xl font-bold mb-10 text-glow text-romantic-900 leading-tight">
            Will you go on a picnic date with me? 🥺❤️
          </h1>
          
          <button 
            @click="emit('next')"
            class="glass-button px-10 py-5 text-xl md:text-2xl w-full font-bold shadow-romantic-500/50 shadow-lg"
          >
            YES, LET'S PLAN IT 💕
          </button>
        </div>
      </Transition>
      
    </div>
  </div>
</template>
