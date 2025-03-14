<template>
  <div class="container">
    <div v-if="timeLeft > 0" class="countdown">
      <h1>Countdown to September 1, 2025</h1>
      <div class="flip-clock">
        <div class="flip-unit" v-for="(value, label) in timeUnits" :key="label">
          <div class="card">
            <div class="card-face front">{{ value }}</div>
            <div class="card-face back">{{ value }}</div>
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
import { ref, computed, onMounted, onUnmounted } from 'vue';

const targetDate = new Date('2025-09-01T00:00:00Z').getTime();
const imageUrl = ref('https://example.com/default-image.jpg');
const timeLeft = ref(targetDate - Date.now());

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
  background: url('@/assets/partay.jpg') no-repeat center center fixed;
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

.card {
  position: relative;
  width: 80px;
  height: 100px;
  background: black;
  border-radius: 10px;
  overflow: hidden;
  font-size: 2rem;
  font-weight: bold;
  color: white;
  text-align: center;
  line-height: 100px;
  box-shadow: 0px 5px 15px rgba(255, 255, 255, 0.3);
}

.card-face {
  position: absolute;
  width: 100%;
  height: 50%;
  left: 0;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.8);
}

.front {
  top: 0;
}

.back {
  bottom: 0;
}

.label {
  font-size: 1rem;
  margin-top: 5px;
}

@keyframes flip {
  0% {
    transform: rotateX(0deg);
  }
  100% {
    transform: rotateX(-180deg);
  }
}

.card.flip .front {
  animation: flip 0.6s ease-in-out;
}
</style>
