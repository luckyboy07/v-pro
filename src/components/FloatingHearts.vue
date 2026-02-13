<template>
  <div class="floating-hearts">
    <template v-if="useImages">
      <img
        v-for="h in hearts"
        :key="h.id"
        :src="h.image"
        class="heart"
        :style="{
          left: h.left + '%',
          animationDuration: h.duration + 's',
          width: h.size + 'px',
          animationDelay: h.delay + 's'
        }"
        alt="floating heart"
      />
    </template>
    <template v-else>
      <span
        v-for="h in hearts"
        :key="h.id"
        class="heart heart-symbol"
        :style="{
          left: h.left + '%',
          animationDuration: h.duration + 's',
          fontSize: h.size + 'px',
          animationDelay: h.delay + 's'
        }"
      >
        {{ h.symbol }}
      </span>
    </template>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import pic1 from '../assets/pic1.jpg'
import pic2 from '../assets/pic2.jpg'
import pic3 from '../assets/pic3.jpg'
import pic4 from '../assets/pic4.jpg'

const heartImages = [pic1, pic2, pic3, pic4]
const heartSymbols = ['❤️', '💕', '💖', '💗', '💓', '💝']
const hearts = ref([])
const useImages = ref(false)

function addHeart() {
  hearts.value.push({
    id: Date.now() + Math.random(),
    left: Math.random() * 100,
    duration: 6 + Math.random() * 6,
    size: useImages.value ? 50 + Math.random() * 30 : 18 + Math.random() * 18,
    delay: Math.random() * 2,
    image: heartImages[Math.floor(Math.random() * heartImages.length)],
    symbol: heartSymbols[Math.floor(Math.random() * heartSymbols.length)]
  })
  // Remove after animation
  setTimeout(() => hearts.value.shift(), 9000)
}

function switchToImages() {
  useImages.value = true
  hearts.value = [] // Clear existing hearts
  // Add new hearts with images
  for (let i = 0; i < 12; i++) setTimeout(addHeart, i * 300)
  setInterval(addHeart, 1200)
}

onMounted(() => {
  for (let i = 0; i < 12; i++) setTimeout(addHeart, i * 300)
  setInterval(addHeart, 1200)
})

defineExpose({ switchToImages })
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
  bottom: -60px;
  opacity: 0.8;
  animation: floatUp 8s linear forwards;
  user-select: none;
}
.heart-symbol {
  filter: drop-shadow(0 2px 4px rgba(255, 105, 180, 0.6));
}
img.heart {
  filter: drop-shadow(0 2px 6px rgba(255, 179, 198, 0.5));
  object-fit: cover;
  border-radius: 6px;
}
@keyframes floatUp {
  0% { transform: translateY(0) scale(1) rotate(0deg);}
  10% { opacity: 1;}
  50% { transform: translateY(-50vh) scale(1.1) rotate(180deg);}
  90% { opacity: 1;}
  100% { transform: translateY(-100vh) scale(1.2) rotate(360deg); opacity: 0;}
}
</style>