<template>
  <div id="app">
    <ScoreBoard :score="score" v-if="gameState !== 'stopped'" />
    <MovingSphere
      v-if="gameState === 'playing'"
      @update-score="updateScore"
      @game-over="handleGameOver"
    />
    <StartButton v-if="gameState === 'stopped'" @start-game="startGame" />
    <GameOver
      v-if="gameState === 'over'"
      :finalScore="score"
      @restart-game="restartGame"
    />
  </div>
</template>

<script>
import StartButton from './components/StartButton.vue';
import ScoreBoard from './components/ScoreBoard.vue';
import MovingSphere from './components/MovingSphere.vue';
import GameOver from './components/GameOver.vue';

export default {
  components: {
    StartButton,
    ScoreBoard,
    MovingSphere,
    GameOver
  },
  data() {
    return {
      gameState: 'stopped',
      score: 0,
    };
  },
  methods: {
    startGame() {
      this.gameState = 'playing';
    },
    updateScore() {
      this.score += 1;
    },
    handleGameOver() {
      this.gameState = 'over';
    },
    restartGame() {
      this.score = 0;
      this.gameState = 'stopped';
    },
  },
};
</script>
