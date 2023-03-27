<template>
  <div class="timer" :class="{ blinking: isBlinking }">
    Time left: {{ timeLeft }}s
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeLeft: 60,
      timerInterval: null,
      isBlinking: false,
    };
  },
  mounted() {
    this.startTimer();
  },
  beforeDestroy() {
    clearInterval(this.timerInterval);
  },
  methods: {
    startTimer() {
      this.timerInterval = setInterval(() => {
        this.timeLeft--;

        if (this.timeLeft <= 10) {
          this.isBlinking = true;
        }

        if (this.timeLeft <= 0) {
          clearInterval(this.timerInterval);
        }
      }, 1000);
    },
  },
};
</script>

<style>
.timer {
  position: absolute;
  top: 10px;
  left: 10px;
  font-size: 24px;
  color: #4caf50;
}

.timer.blinking {
  animation: blink 1s linear infinite;
}

@keyframes blink {
  0%,
  100% {
    color: #4caf50;
  }
  50% {
    color: red;
  }
}
</style>
