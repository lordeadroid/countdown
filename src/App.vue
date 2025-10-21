<script setup>
import { ref, onMounted, onUnmounted, watchEffect } from "vue";

const now = ref(new Date());
const startTime = new Date("2025-07-19T00:00:00.000+05:30").getTime();
const days = ref(0);
const hours = ref(0);
const minutes = ref(0);
const seconds = ref(0);

const intervalId = ref(null);

watchEffect(() => {
  const remaining = now.value.getTime() - startTime;
  days.value = Math.floor(remaining / (1000 * 60 * 60 * 24));
  hours.value = Math.floor(
    (remaining % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
  );
  minutes.value = Math.floor((remaining % (1000 * 60 * 60)) / (1000 * 60));
  seconds.value = Math.floor((remaining % (1000 * 60)) / 1000);
});

onMounted(() => {
  intervalId.value = setInterval(() => {
    now.value = new Date();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(intervalId.value);
});
</script>

<template>
  <div>
    <h1>Countdown Timer</h1>
    <div>
      <span>{{ days }} Days</span>
      <span>{{ hours }} Hours</span>
      <span>{{ minutes }} Minutes</span>
      <span>{{ seconds }} Seconds</span>
    </div>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
}
h1 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
span {
  font-size: 1.5rem;
}
</style>
