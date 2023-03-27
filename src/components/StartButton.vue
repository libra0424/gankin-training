<template>
  <button @click="startGame" class="start-button">
    Start Game
  </button>
</template>

<script>
export default {
  methods: {
    startGame() {
      this.$emit('start-game');
    },
  },
};
</script>

<style>
.start-button {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  cursor: pointer;
}
</style>