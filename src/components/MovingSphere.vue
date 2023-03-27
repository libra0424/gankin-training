<template>
  <div
    class="sphere"
    :style="{ left: position.x + 'px', top: position.y + 'px' }"
    ref="sphere"
  >
    <span class="letter" :class="{ breaking: isBreaking }">{{ letter }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      position: {
        x: 0,
        y: 0,
      },
      direction: {
        x: 1,
        y: 1,
      },
      letter: "",
      alphabet: ["a", "s", "d"],
      speed: 5,
      gameDuration: 60000,
      isBreaking: false,
      gameInterval: null,
      speedIncreaseInterval: null,
    };
  },
  mounted() {
    this.initializeSphere();
    this.gameInterval = setInterval(this.moveSphere, 20);
    this.speedIncreaseInterval = setInterval(this.increaseSpeed, 10000);
    this.setGameDuration();
    this.setKeyListener();
  },
  beforeDestroy() {
    clearInterval(this.gameInterval);
    clearInterval(this.speedIncreaseInterval);
    window.removeEventListener("keydown", this.keydownHandler);
  },
  methods: {
    initializeSphere() {
      // 球の始点位置をウィンドウの中央付近のランダムな位置に設定します
      this.position = {
        x: Math.floor(Math.random() * window.innerWidth),
        y: Math.floor(Math.random() * window.innerHeight),
      };
      // 四方向キーからランダムにアルファベットを選びます
      this.letter = this.getRandomLetter(this.alphabet);
    },
    moveSphere() {
      // 衝突のチェック
      this.checkForCollision();
      // スピードを加算,最初はランダムな方向に移動するようにする
      this.position.x += this.direction.x * this.speed;
      this.position.y += this.direction.y * this.speed;
    },
    increaseSpeed() {
      // スピードを1加算
      this.speed += 1;
    },
    checkForCollision() {
      // 球体DOM要素を取得して、範囲を取得
      const sphere = this.$refs.sphere;
      const sphereRect = sphere.getBoundingClientRect();
      // 範囲比較による方向の反転
      if (sphereRect.left <= 0 || sphereRect.right >= window.innerWidth) {
        this.direction.x *= -1;
      }
      if (sphereRect.top <= 0 || sphereRect.bottom >= window.innerHeight) {
        this.direction.y *= -1;
      }
    },
    setGameDuration() {
      // ゲーム時間設定
      setTimeout(() => {
        clearInterval(this.gameInterval);
        clearInterval(this.speedIncreaseInterval);
        this.$emit("game-over");
      }, this.gameDuration);
    },
    setKeyListener() {
      // キー入力監視をセット
      window.addEventListener("keydown", this.keydownHandler);
    },
    keydownHandler(event) {
      if (this.alphabet.includes(event.key)) {
        // 入力された文字列が適切かチェックし、得点を追加
        if (event.key === this.letter) {
          this.$emit("update-score");
          // 文字が砕け散るアニメーションを実行
          this.breakingAnimation();
          // 次の出題のための配列を作成、alphabetからletterを除外する
          const letters = this.alphabet.filter((letter) => letter !== this.letter);
          this.letter = this.getRandomLetter(letters);
        }
      }
    },
    getRandomLetter(letters) {
      // 指定された文字列の中からランダムに文字を得る
      return letters[Math.floor(Math.random() * letters.length)];
    },

    breakingAnimation() {
      this.isBreaking = true;
      setTimeout(() => {
        this.isBreaking = false;
      }, 200);
    },
  },
};
</script>

<style>
.sphere {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  background-color: #4caf50;
  border-radius: 50%;
  color: white;
  font-size: 45px;
  user-select: none;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}
.letter {
  position: relative;
  display: inline-block;
}

.letter.breaking {
  animation: breaking 0.5s forwards;
}

@keyframes breaking {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(3);
    color: yellow;
    opacity: 1;
  }
  100% {
    transform: scale(1);
    opacity: 0;
  }
}
</style>