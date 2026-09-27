<script setup>
import { ref, onMounted } from 'vue';

const emit = defineEmits(['next']);

const showConfirmText = ref(false);
const showTransitionText = ref(false);
const showLetter = ref(false);

onMounted(() => {
  setTimeout(() => {
    showConfirmText.value = true;
  }, 500);

  setTimeout(() => {
    showConfirmText.value = false;
  }, 3000);
  
  setTimeout(() => {
    showTransitionText.value = true;
  }, 4000);

  setTimeout(() => {
    showTransitionText.value = false;
  }, 7000);

  setTimeout(() => {
    showLetter.value = true;
  }, 8000);
});

</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen p-6 z-10 relative">
    <div class="max-w-2xl w-full text-center">
      
      <Transition name="fade">
        <h2 v-if="showConfirmText" class="text-4xl md:text-5xl font-bold text-romantic-900 text-glow">
          DATE CONFIRMED ❤️
        </h2>
      </Transition>

      <Transition name="fade">
        <p v-if="showTransitionText" class="text-2xl md:text-3xl font-medium text-slate-700">
          Okay... now for the part I actually wanted you to see.
        </p>
      </Transition>

      <Transition name="slide-up">
        <div v-if="showLetter" class="glass-card p-8 md:p-12 text-left relative overflow-hidden">
          <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-romantic-300 via-romantic-500 to-romantic-300"></div>
          
          <div class="space-y-6 text-lg md:text-xl text-slate-700 font-sans leading-relaxed">
            <p class="animate-fade-in" style="animation-delay: 0.5s; opacity: 0; animation-fill-mode: forwards;">
              I don't think I tell you enough just how much you mean to me.
            </p>
            
            <p class="animate-fade-in" style="animation-delay: 2s; opacity: 0; animation-fill-mode: forwards;">
              You're not just someone I love spending time with. You're someone who makes ordinary days feel special.
            </p>
            
            <p class="animate-fade-in" style="animation-delay: 4.5s; opacity: 0; animation-fill-mode: forwards;">
              I love your smile, your personality, your little ways of doing things, the conversations we have, and even the silly things that somehow become our favorite memories.
            </p>
            
            <p class="animate-fade-in" style="animation-delay: 7s; opacity: 0; animation-fill-mode: forwards;">
              I don't need everything to be perfect. I just want more moments with you.
            </p>
            
            <div class="pl-4 border-l-4 border-romantic-300 space-y-2 italic animate-fade-in text-romantic-800" style="animation-delay: 9s; opacity: 0; animation-fill-mode: forwards;">
              <p>More laughs.</p>
              <p>More conversations.</p>
              <p>More random adventures.</p>
              <p>More memories.</p>
              <p>More us.</p>
            </div>
            
            <p class="animate-fade-in font-medium pt-4" style="animation-delay: 11.5s; opacity: 0; animation-fill-mode: forwards;">
              And yes...
            </p>
            
            <p class="animate-fade-in text-2xl font-bold text-romantic-900" style="animation-delay: 13s; opacity: 0; animation-fill-mode: forwards;">
              I really, really love you. ❤️
            </p>
          </div>

          <div class="mt-12 text-center animate-fade-in" style="animation-delay: 15s; opacity: 0; animation-fill-mode: forwards;">
            <button 
              @click="emit('next')"
              class="glass-button px-8 py-3 w-full md:w-auto"
            >
              Continue ❤️
            </button>
          </div>
        </div>
      </Transition>
      
    </div>
  </div>
</template>

<style scoped>
.animate-fade-in {
  animation: smoothFadeIn 1.5s ease-out forwards;
}

@keyframes smoothFadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
