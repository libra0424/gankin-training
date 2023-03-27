<template>
  <div class="scoreboard">
    <span>Score:</span>
    <span>{{ score }}</span>
  </div>
</template>

<script>
export default {
  props: {
    score: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style>
.scoreboard {
  position: absolute;
  top: 10px;
  right: 10px;
  display: flex;
  align-items: center;
  font-size: 24px;
  color: #4caf50;
}

.scoreboard span {
  margin-right: 5px;
}
</style>