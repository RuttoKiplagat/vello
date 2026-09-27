<script setup>
import { ref, computed } from 'vue';
import { 
  addMonths, 
  startOfMonth, 
  endOfMonth, 
  eachDayOfInterval, 
  format, 
  isSameMonth, 
  isSameDay, 
  startOfWeek, 
  endOfWeek,
  isBefore,
  startOfToday
} from 'date-fns';
import confetti from 'canvas-confetti';
import { Check, Calendar as CalendarIcon, Sparkles } from 'lucide-vue-next';

const emit = defineEmits(['confirm']);

// State
const step = ref(1);
const selectedDate = ref(null);
const selectedType = ref(null);
const selectedItems = ref([]);

// Calendar Logic (Next Month)
const today = startOfToday();
const nextMonth = startOfMonth(addMonths(today, 1));
const calendarEnd = endOfMonth(nextMonth);

const monthName = format(nextMonth, 'MMMM yyyy');

const calendarDays = computed(() => {
  const start = startOfWeek(nextMonth);
  const end = endOfWeek(calendarEnd);
  return eachDayOfInterval({ start, end });
});

const isSelectable = (date) => {
  return isSameMonth(date, nextMonth);
};

const selectDate = (date) => {
  if (isSelectable(date)) {
    selectedDate.value = date;
    setTimeout(() => {
      step.value = 2;
    }, 600);
  }
};

const formattedSelectedDate = computed(() => {
  if (!selectedDate.value) return '';
  return format(selectedDate.value, 'EEEE, MMMM do, yyyy');
});

// Picnic Types
const picnicTypes = [
  { id: 'cute', label: '🌸 Cute & Romantic' },
  { id: 'food', label: '🍕 Food + Chill' },
  { id: 'games', label: '🎲 Games + Laughing' },
  { id: 'sunset', label: '🌅 Sunset Picnic' },
  { id: 'surprise', label: '🥹 Surprise Me' }
];

const selectType = (type) => {
  selectedType.value = type;
  setTimeout(() => {
    step.value = 3;
  }, 600);
};

// Picnic Items
const picnicItemsList = [
  { id: 'snacks', label: '🍓 Snacks' },
  { id: 'chocolate', label: '🍫 Chocolate' },
  { id: 'drinks', label: '🧃 Drinks' },
  { id: 'flowers', label: '🌹 Flowers' },
  { id: 'everything', label: '🧺 Everything!' }
];

const toggleItem = (item) => {
  if (item.id === 'everything') {
    if (selectedItems.value.some(i => i.id === 'everything')) {
      selectedItems.value = [];
    } else {
      selectedItems.value = [...picnicItemsList];
    }
    return;
  }
  
  // Remove 'everything' if selecting individual items
  selectedItems.value = selectedItems.value.filter(i => i.id !== 'everything');
  
  const index = selectedItems.value.findIndex(i => i.id === item.id);
  if (index === -1) {
    selectedItems.value.push(item);
  } else {
    selectedItems.value.splice(index, 1);
  }
};

const confirmDate = () => {
  // Fire confetti
  const duration = 3000;
  const end = Date.now() + duration;

  const frame = () => {
    confetti({
      particleCount: 5,
      angle: 60,
      spread: 55,
      origin: { x: 0 },
      colors: ['#f472b6', '#db2777', '#9d174d']
    });
    confetti({
      particleCount: 5,
      angle: 120,
      spread: 55,
      origin: { x: 1 },
      colors: ['#f472b6', '#db2777', '#9d174d']
    });

    if (Date.now() < end) {
      requestAnimationFrame(frame);
    }
  };
  frame();

  setTimeout(() => {
    emit('confirm', {
      date: formattedSelectedDate.value,
      type: selectedType.value,
      items: selectedItems.value
    });
  }, 3500);
};

</script>

<template>
  <div class="w-full max-w-2xl mx-auto z-10 relative py-8 px-4">
    
    <!-- Step 1: Calendar -->
    <Transition name="fade" mode="out-in">
      <div v-if="step === 1" class="glass-card p-6 md:p-8">
        <div class="text-center mb-8">
          <CalendarIcon class="w-10 h-10 text-romantic-500 mx-auto mb-4" />
          <h2 class="text-2xl md:text-3xl font-bold text-romantic-900 mb-2">When should we go?</h2>
          <p class="text-slate-600">Pick a date for our picnic ❤️</p>
        </div>

        <div class="mb-6">
          <h3 class="text-xl font-semibold text-center text-romantic-700 mb-4">{{ monthName }}</h3>
          
          <div class="grid grid-cols-7 gap-1 md:gap-2 mb-2 text-center text-sm font-medium text-slate-500">
            <div v-for="day in ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa']" :key="day">{{ day }}</div>
          </div>
          
          <div class="grid grid-cols-7 gap-1 md:gap-2">
            <button
              v-for="date in calendarDays"
              :key="date.toISOString()"
              @click="selectDate(date)"
              :disabled="!isSelectable(date)"
              class="aspect-square flex items-center justify-center rounded-lg text-sm transition-all relative overflow-hidden"
              :class="[
                !isSelectable(date) ? 'text-slate-300 cursor-not-allowed' : 'hover:bg-romantic-100 hover:text-romantic-800 text-slate-700',
                selectedDate && isSameDay(date, selectedDate) ? 'bg-romantic-500 text-white font-bold shadow-md transform scale-105' : 'bg-white/40'
              ]"
            >
              {{ format(date, 'd') }}
              <Heart 
                v-if="selectedDate && isSameDay(date, selectedDate)" 
                class="absolute w-full h-full p-2 text-white/20 animate-pulse" 
              />
            </button>
          </div>
        </div>
      </div>

      <!-- Step 2: Picnic Type -->
      <div v-else-if="step === 2" class="glass-card p-6 md:p-8">
        <div class="text-center mb-8">
          <h2 class="text-xl text-slate-600 mb-2">Our Picnic Date ❤️</h2>
          <p class="text-2xl font-bold text-romantic-800 mb-6">{{ formattedSelectedDate }}</p>
          <h3 class="text-2xl md:text-3xl font-bold text-romantic-900">What kind of picnic should we have? 🥰</h3>
        </div>

        <div class="space-y-3">
          <button
            v-for="ptype in picnicTypes"
            :key="ptype.id"
            @click="selectType(ptype)"
            class="w-full p-4 rounded-xl border text-left transition-all duration-300"
            :class="[
              selectedType && selectedType.id === ptype.id 
                ? 'bg-romantic-100 border-romantic-500 shadow-md text-romantic-900 scale-[1.02]' 
                : 'bg-white/60 border-white/60 hover:bg-white/90 hover:border-romantic-300'
            ]"
          >
            <span class="text-lg font-medium">{{ ptype.label }}</span>
          </button>
        </div>
      </div>

      <!-- Step 3 & 4: Items & Confirmation -->
      <div v-else-if="step >= 3" class="glass-card p-6 md:p-8">
        <div v-if="step === 3">
          <div class="text-center mb-8">
            <h2 class="text-2xl md:text-3xl font-bold text-romantic-900 mb-2">Okay... what should I bring? 👀</h2>
            <p class="text-slate-600">Select all you want!</p>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-3 mb-8">
            <button
              v-for="item in picnicItemsList"
              :key="item.id"
              @click="toggleItem(item)"
              class="p-4 rounded-xl border flex items-center justify-between transition-all duration-300"
              :class="[
                selectedItems.some(i => i.id === item.id)
                  ? 'bg-romantic-100 border-romantic-500 shadow-sm text-romantic-900' 
                  : 'bg-white/60 border-white/60 hover:bg-white/90 hover:border-romantic-300'
              ]"
            >
              <span class="text-lg font-medium">{{ item.label }}</span>
              <div 
                class="w-6 h-6 rounded-full border-2 flex items-center justify-center transition-colors"
                :class="selectedItems.some(i => i.id === item.id) ? 'bg-romantic-500 border-romantic-500' : 'border-slate-300'"
              >
                <Check v-if="selectedItems.some(i => i.id === item.id)" class="w-4 h-4 text-white" />
              </div>
            </button>
          </div>

          <button 
            @click="step = 4"
            class="glass-button w-full py-4 text-lg"
            :disabled="selectedItems.length === 0"
          >
            Looks good to me ❤️
          </button>
        </div>

        <div v-if="step === 4" class="text-center">
          <Sparkles class="w-12 h-12 text-romantic-500 mx-auto mb-4 animate-pulse" />
          <h2 class="text-3xl font-bold text-romantic-900 mb-8">Our Little Date ❤️</h2>
          
          <div class="bg-white/50 rounded-xl p-6 mb-8 text-left space-y-4 shadow-sm border border-white/60">
            <div>
              <p class="text-sm text-slate-500 font-medium uppercase tracking-wider mb-1">Date</p>
              <p class="text-lg font-semibold text-slate-800">{{ formattedSelectedDate }}</p>
            </div>
            <div>
              <p class="text-sm text-slate-500 font-medium uppercase tracking-wider mb-1">Picnic Style</p>
              <p class="text-lg font-semibold text-slate-800">{{ selectedType.label }}</p>
            </div>
            <div>
              <p class="text-sm text-slate-500 font-medium uppercase tracking-wider mb-1">I'll bring</p>
              <p class="text-lg font-semibold text-slate-800">
                {{ selectedItems.map(i => i.label).join(', ') }}
              </p>
            </div>
          </div>

          <h3 class="text-xl font-bold text-romantic-800 mb-6">Confirm our little date? ❤️</h3>
          
          <button 
            @click="confirmDate"
            class="glass-button w-full py-5 text-xl font-bold shadow-lg"
          >
            YES, IT'S A DATE 🥹❤️
          </button>
        </div>
      </div>
    </Transition>
  </div>
</template>
