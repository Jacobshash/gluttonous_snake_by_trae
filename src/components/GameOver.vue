<template>
  <div class="game-over">
    <h2>游戏结束！</h2>
    <p>得分：<span class="score">{{ score }}</span></p>
    <button @click="emitRestart">重新开始</button>
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
  background: rgba(0,0,0,0.8);
  color: white;
  padding: 2rem;
  text-align: center;
  border-radius: 10px;
}

button {
  background: #4CAF50;
  border: none;
  padding: 0.8rem 1.5rem;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 1rem;
}

button:hover {
  background: #45a049;
}
</style>