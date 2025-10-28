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
  <div className="body">
    <div>CountDown</div>
    <div className="content">
      <span>{{ days }} Day</span>
      <span>{{ hours }} Hour</span>
      <span>{{ minutes }} Minute</span>
      <span>{{ seconds }} Second</span>
    </div>
  </div>
</template>

<style scoped>
.body {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  height: 100dvh;
  width: 100dvw;
  font-size: 5rem;
  font-style: italic;
  font-weight: 900;
  color: teal;

  & > .content {
    display: flex;
    align-items: center;
    flex-direction: column;
    background: linear-gradient(180deg, #ff0080, #ff8c00, #40e0d0, #ff0080);
    background-size: 500% 500%;
    animation: move-gradient 20s ease-in infinite;
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
}

@keyframes move-gradient {
  0% {
    background-position: 50% 0%;
  }
  50% {
    background-position: 50% 100%;
  }
  100% {
    background-position: 50% 0%;
  }
}
</style>
