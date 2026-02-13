<template>
  <div class="floating-hearts">
    <span
      v-for="h in hearts"
      :key="h.id"
      class="heart"
      :style="{
        left: h.left + '%',
        animationDuration: h.duration + 's',
        fontSize: h.size + 'px',
        animationDelay: h.delay + 's'
      }"
    >{{ h.symbol }}</span>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const heartSymbols = ['💖','❤️','💕','💗','💓','💞']
const hearts = ref([])

function addHeart() {
  hearts.value.push({
    id: Date.now() + Math.random(),
    left: Math.random() * 100,
    duration: 6 + Math.random() * 6,
    size: 18 + Math.random() * 18,
    delay: Math.random() * 2,
    symbol: heartSymbols[Math.floor(Math.random() * heartSymbols.length)]
  })
  // Remove after animation
  setTimeout(() => hearts.value.shift(), 9000)
}

onMounted(() => {
  for (let i = 0; i < 12; i++) setTimeout(addHeart, i * 300)
  setInterval(addHeart, 1200)
})
</script>

<style scoped>
.floating-hearts {
  position: fixed;
  top: 0; left: 0;
  width: 100vw; height: 100vh;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}
.heart {
  position: absolute;
  bottom: -40px;
  opacity: 0.7;
  animation: floatUp 8s linear forwards;
  user-select: none;
  filter: drop-shadow(0 2px 6px #ffb3c6);
}
@keyframes floatUp {
  0% { transform: translateY(0) scale(1);}
  10% { opacity: 1;}
  90% { opacity: 1;}
  100% { transform: translateY(-100vh) scale(1.2); opacity: 0;}
}
</style>