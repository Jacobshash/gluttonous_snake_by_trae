<template>
  <div class="game-over">
    <h2>🎮 游戏结束！</h2>
    <p>🏆 得分：<span class="score">{{ score }}</span></p>
    <button @click="emitRestart">🔄 再玩一次</button>
  </div>
</template>

<script setup>
const props = defineProps({
  score: {
    type: Number,
    default: 0
  }
});

const emit = defineEmits(['restart']);

const emitRestart = () => {
  emit('restart');
};
</script>

<style scoped>
.game-over {
  position: absolute;
  background: linear-gradient(145deg, #ff9a9e, #fad0c4);
  border-radius: 20px;
  padding: 2.5rem;
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
  border: 4px solid #fff;
  animation: bounceIn 0.6s ease-out;
  font-family: 'Bangers', cursive;
}

@keyframes bounceIn {
  0% { transform: scale(0); }
  60% { transform: scale(1.1); }
  80% { transform: scale(0.9); }
  100% { transform: scale(1); }
}

button {
  background: linear-gradient(45deg, #ffd93d, #ff6b6b);
  border: none;
  padding: 1rem 2rem;
  font-size: 1.2rem;
  border-radius: 30px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  transition: all 0.3s ease;
  font-family: 'Bangers', cursive;
  letter-spacing: 1px;
}

button:hover {
  transform: scale(1.05) rotate(3deg);
  box-shadow: 0 6px 12px rgba(0,0,0,0.3);
}
</style>