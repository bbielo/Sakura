<template>
  <canvas ref="canvasRef" class="sakura-canvas"></canvas>
</template>

<script setup>
import { onMounted, onBeforeUnmount, ref } from "vue";

const canvasRef = ref(null);

let canvas = null;
let ctx = null;
let animationId = null;

const petals = ["🌸", "✿", "❀", "💮"];
const sparkles = ["✨", "⋆", "✦"];

let blossoms = [];
let twinkles = [];

function resizeCanvas() {
  if (!canvas) return;
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

function createItems() {
  if (!canvas) return;

  blossoms = Array.from({ length: 50 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    speed: Math.random() * 2 + 0.5,
    drift: Math.random() * 1.2 - 0.6,
    shape: petals[Math.floor(Math.random() * petals.length)],
  }));

  twinkles = Array.from({ length: 20 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    shape: sparkles[Math.floor(Math.random() * sparkles.length)],
  }));
}

function draw() {
  if (!ctx || !canvas) return;

  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.font = "20px serif";

  twinkles.forEach((t) => {
    if (Math.random() < 0.04) {
      t.x = Math.random() * canvas.width;
      t.y = Math.random() * canvas.height;
      t.shape = sparkles[Math.floor(Math.random() * sparkles.length)];
    }
    ctx.fillText(t.shape, t.x, t.y);
  });

  blossoms.forEach((b) => {
    ctx.fillText(b.shape, b.x, b.y);

    b.y += b.speed;
    b.x += b.drift + (Math.random() - 0.5) * 0.5;

    if (b.y > canvas.height + 20) {
      b.y = -20;
      b.x = Math.random() * canvas.width;
      b.shape = petals[Math.floor(Math.random() * petals.length)];
      b.speed = Math.random() * 2 + 0.5;
    }
  });

  animationId = requestAnimationFrame(draw);
}

function handleResize() {
  resizeCanvas();
  createItems();
}

onMounted(() => {
  canvas = canvasRef.value;
  if (!canvas) return;

  ctx = canvas.getContext("2d");
  resizeCanvas();
  createItems();
  draw();

  window.addEventListener("resize", handleResize);
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", handleResize);
  if (animationId) cancelAnimationFrame(animationId);
});
</script>

<style scoped>
.sakura-canvas {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}
</style>