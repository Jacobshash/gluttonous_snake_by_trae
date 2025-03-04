<template>
  <div class="game-container">
    <canvas ref="canvas" :width="canvasSize" :height="canvasSize"></canvas>
    <GameOver 
      v-if="isGameOver"
      :score="score"
      @restart="restartGame"
    />
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue';
// 这里假定修改 GameOver 组件的模板以符合单根元素要求
// 假设 GameOver 组件模板修改为包裹在一个根元素中
import GameOver from './components/GameOver.vue';

const canvasSize = 400;
const gridSize = 20;
const tileCount = canvasSize / gridSize;

const canvas = ref(null);
const ctx = ref(null);
const gameLoop = ref(null);

// 响应式游戏状态
const snake = reactive([{ x: 10, y: 10 }]);
const food = reactive({ x: 15, y: 15 });
const direction = reactive({ dx: 1, dy: 0 });
const score = ref(0);
const isGameOver = ref(false);

// 初始化画布
onMounted(() => {
  ctx.value = canvas.value.getContext('2d');
  startGame();
  window.addEventListener('keydown', handleKeyPress);
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyPress);
  clearInterval(gameLoop.value);
});

// 游戏主循环
const drawGame = () => {
  ctx.value.fillStyle = 'white';
  ctx.value.fillRect(0, 0, canvasSize, canvasSize);

  // 移动蛇头
  const head = { 
    x: snake[0].x + direction.dx, 
    y: snake[0].y + direction.dy 
  };
  snake.unshift(head);

  // 吃食物检测
  if (head.x === food.x && head.y === food.y) {
    score.value += 10;
    generateFood();
  } else {
    snake.pop();
  }

  // 绘制食物
  ctx.value.fillStyle = 'red';
  ctx.value.fillRect(food.x * gridSize, food.y * gridSize, gridSize-2, gridSize-2);

  // 绘制蛇身
  ctx.value.fillStyle = '#4CAF50';
  snake.forEach(segment => {
    ctx.value.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize-2, gridSize-2);
  });

  // 碰撞检测
  if (isCollision(head)) {
    gameOver();
  }
};

const generateFood = () => {
  food.x = Math.floor(Math.random() * tileCount);
  food.y = Math.floor(Math.random() * tileCount);
};

const isCollision = (head) => {
  if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
    return true;
  }
  return snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y);
};

const gameOver = () => {
  clearInterval(gameLoop.value);
  isGameOver.value = true;
};

const startGame = () => {
  gameLoop.value = setInterval(drawGame, 100);
};

const restartGame = () => {
  snake.splice(0, snake.length, { x: 10, y: 10 });
  Object.assign(food, { x: 15, y: 15 });
  Object.assign(direction, { dx: 1, dy: 0 });
  score.value = 0;
  isGameOver.value = false;
  startGame();
};

// 键盘控制
const handleKeyPress = (e) => {
  switch(e.key) {
    case 'ArrowUp':
    case 'w':
      if (direction.dy !== 1) {
        direction.dx = 0;
        direction.dy = -1;
      }
      break;
    case 'ArrowDown':
    case 's':
      if (direction.dy !== -1) {
        direction.dx = 0;
        direction.dy = 1;
      }
      break;
    case 'ArrowLeft':
    case 'a':
      if (direction.dx !== 1) {
        direction.dx = -1;
        direction.dy = 0;
      }
      break;
    case 'ArrowRight':
    case 'd':
      if (direction.dx !== -1) {
        direction.dx = 1;
        direction.dy = 0;
      }
      break;
  }
};
</script>

<style scoped>
.game-container {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}

canvas {
  border: 2px solid #4CAF50;
  border-radius: 5px;
  background-color: #f0f0f0;
}
</style>