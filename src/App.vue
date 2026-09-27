<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { Volume2, VolumeX, Heart } from 'lucide-vue-next';
import { questions } from './data/questions';

import Welcome from './components/Welcome.vue';
import ProgressBar from './components/ProgressBar.vue';
import QuestionCard from './components/QuestionCard.vue';
import TransitionScreen from './components/TransitionScreen.vue';
import PicnicPlanner from './components/PicnicPlanner.vue';
import LoveLetter from './components/LoveLetter.vue';
import FinalReveal from './components/FinalReveal.vue';

// State
const stage = ref('welcome');
const currentQuestionIndex = ref(0);
const answers = ref([]);
const picnicData = ref(null);
const isMusicPlaying = ref(false);

const audioRef = ref(null);

// Stages: welcome, questions, transition, planner, letter, finale

// Navigation
const startExperience = () => {
  stage.value = 'questions';
  if (isMusicPlaying.value && audioRef.value) {
    audioRef.value.play().catch(e => console.log('Audio play failed', e));
  }
};

const handleAnswer = (answer) => {
  answers.value.push(answer);
  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++;
  } else {
    stage.value = 'transition';
  }
};

const handleTransitionNext = () => {
  stage.value = 'planner';
};

const handlePicnicConfirm = (data) => {
  picnicData.value = data;
  stage.value = 'letter';
};

const handleLetterNext = () => {
  stage.value = 'finale';
};

const restart = () => {
  stage.value = 'welcome';
  currentQuestionIndex.value = 0;
  answers.value = [];
  picnicData.value = null;
};

// Background Music Toggle
const toggleMusic = () => {
  isMusicPlaying.value = !isMusicPlaying.value;
  if (audioRef.value) {
    if (isMusicPlaying.value) {
      audioRef.value.play().catch(e => {
        console.log('Audio play failed', e);
        isMusicPlaying.value = false;
      });
    } else {
      audioRef.value.pause();
    }
  }
};

// Floating Hearts Background Generation
const hearts = ref([]);
const createHearts = () => {
  for (let i = 0; i < 20; i++) {
    hearts.value.push({
      id: i,
      left: Math.random() * 100,
      animationDuration: 15 + Math.random() * 15,
      delay: Math.random() * 10,
      size: 0.5 + Math.random() * 1.5,
      opacity: 0.1 + Math.random() * 0.3
    });
  }
};

onMounted(() => {
  createHearts();
});
</script>

<template>
  <div class="min-h-screen w-full relative overflow-hidden bg-gradient-to-br from-pink-50 via-purple-50 to-pink-100 font-sans text-slate-800">
    
    <!-- Background Floating Hearts -->
    <div class="fixed inset-0 pointer-events-none z-0 overflow-hidden" aria-hidden="true">
      <Heart 
        v-for="heart in hearts" 
        :key="heart.id"
        class="absolute bottom-0 text-romantic-300"
        :style="{
          left: `${heart.left}%`,
          width: `${heart.size}rem`,
          height: `${heart.size}rem`,
          opacity: heart.opacity,
          animation: `float ${heart.animationDuration}s linear infinite`,
          animationDelay: `${heart.delay}s`
        }"
      />
    </div>

    <!-- Optional Music Player UI (only show on welcome) -->
    <div v-if="stage === 'welcome'" class="fixed top-6 right-6 z-50">
      <button 
        @click="toggleMusic"
        class="p-3 glass-card rounded-full hover:bg-white/90 transition-colors text-romantic-600 shadow-sm"
        aria-label="Toggle background music"
      >
        <Volume2 v-if="isMusicPlaying" class="w-5 h-5" />
        <VolumeX v-else class="w-5 h-5" />
      </button>
      
      <!-- We use a placeholder audio source since one wasn't provided, 
           but the mechanism works. The user can swap the src later. -->
      <audio ref="audioRef" loop>
        <source src="https://assets.mixkit.co/music/preview/mixkit-romantic-piano-136.mp3" type="audio/mpeg">
      </audio>
    </div>

    <!-- Main Content Area -->
    <div class="relative z-10 flex flex-col min-h-screen">
      
      <Transition name="fade" mode="out-in">
        <Welcome v-if="stage === 'welcome'" @start="startExperience" />
        
        <div v-else-if="stage === 'questions'" class="flex flex-col items-center justify-center flex-1 p-4 w-full">
          <ProgressBar 
            :current="currentQuestionIndex + 1" 
            :total="questions.length" 
          />
          <Transition name="slide-up" mode="out-in">
            <QuestionCard 
              :key="currentQuestionIndex"
              :question="questions[currentQuestionIndex]"
              @answer="handleAnswer"
            />
          </Transition>
        </div>
        
        <TransitionScreen v-else-if="stage === 'transition'" @next="handleTransitionNext" />
        
        <PicnicPlanner v-else-if="stage === 'planner'" @confirm="handlePicnicConfirm" />
        
        <LoveLetter v-else-if="stage === 'letter'" @next="handleLetterNext" />
        
        <FinalReveal 
          v-else-if="stage === 'finale'" 
          :selectedDate="picnicData?.date || ''" 
          @restart="restart" 
        />
      </Transition>
      
    </div>
  </div>
</template>
