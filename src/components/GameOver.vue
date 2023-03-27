<template>
  <div class="game-over">
    <h2>Game Over</h2>
    <p>Your final score: {{ finalScore }}</p>
    <button @click="restartGame" class="restart-button">
      Restart Game
    </button>
  </div>
</template>

<script>
export default {
  props: {
    finalScore: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    restartGame() {
      this.$emit("restart-game");
    },
  },
};
</script>

<style>
.game-over {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

.restart-button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  cursor: pointer;
  margin-top: 20px;
}
</style>
