<template>
  <div class="container">
    <p class="quote" style="font-style: italic">"Where we're going, we don't need roads."</p>
    <div v-if="timeLeft > 0" class="countdown">
      <h1>Countdown to September 1, 2025</h1>
      <div class="flip-clock">
        <div class="flip-unit" v-for="(value, label) in timeUnits" :key="label">
          <div class="card-wrapper">
            <transition name="slide">
              <div class="card" :key="value">
                <div class="front">{{ value }}</div>
              </div>
            </transition>
          </div>
          <span class="label">{{ label }}</span>
        </div>
      </div>
    </div>
    <div v-else class="image-display">
      <img :src="imageUrl" alt="Revealed Image" />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch } from 'vue';

const targetDate = new Date('2025-09-01T00:00:00Z').getTime();
const imageUrl = ref('/byefelicia.jpg');
const timeLeft = ref(targetDate - Date.now());
const previousTimeUnits = ref({ Days: 0, Hours: 0, Minutes: 0, Seconds: 0 });

const updateCountdown = () => {
  timeLeft.value = targetDate - Date.now();
};

const timeUnits = computed(() => {
  const totalSeconds = Math.floor(timeLeft.value / 1000);
  return {
    Days: Math.floor(totalSeconds / (3600 * 24)),
    Hours: Math.floor((totalSeconds % (3600 * 24)) / 3600),
    Minutes: Math.floor((totalSeconds % 3600) / 60),
    Seconds: totalSeconds % 60
  };
});

watch(timeUnits, (newValues) => {
  for (const label in newValues) {
    if (newValues[label] !== previousTimeUnits.value[label]) {
      previousTimeUnits.value[label] = newValues[label];
    }
  }
});

let interval;

onMounted(() => {
  interval = setInterval(updateCountdown, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300;400;700&display=swap');

body {
  margin: 0;
  padding: 0;
  font-family: 'Nunito', sans-serif;
  background: url('../../public/partay.jpg') no-repeat center center fixed;
  background-size: cover;
  color: white;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  backdrop-filter: blur(5px);
  background-color: rgba(0, 0, 0, 0.5);
  padding: 20px;
  border-radius: 15px;
}

h1 {
  color: white;
  margin-bottom: 20px;
}

.flip-clock {
  display: flex;
  gap: 15px;
}

.flip-unit {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.card-wrapper {
  position: relative;
  width: 80px;
  height: 100px;
  overflow: hidden;
}

.card {
  width: 100%;
  height: 100%;
  background: black;
  border-radius: 10px;
  font-size: 2rem;
  font-weight: bold;
  color: white;
  text-align: center;
  line-height: 100px;
  box-shadow: 0px 5px 15px rgba(255, 255, 255, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  top: 0;
  left: 0;
  transition: transform 0.6s ease-in-out;
}

.slide-enter-active, .slide-leave-active {
  transition: transform 0.6s ease-in-out;
}

.slide-enter {
  transform: translateY(-100%);
}

.slide-leave-to {
  transform: translateY(100%);
}

.label {
  font-size: 1rem;
  margin-top: 5px;
}
</style>
