<template>
  <div class="card">
    <h1>Will you be my Valentine? 💖</h1>
    <p class="message">You make my world brighter every day. I’d be the luckiest to call you my Valentine!</p>
    <div class="buttons" v-if="!accepted">
      <button class="yes" @click="accept">Yes 💕</button>
      <button
        class="no"
        ref="noBtn"
        @mouseover="moveNo"
        @touchstart="moveNo"
        :style="{ left: noPos.x + 'px', top: noPos.y + 'px', position: noMoved ? 'fixed' : 'relative' }"
      >No 😅</button>
    </div>
    <transition name="fade">
      <div v-if="accepted" class="final-message">
        <div class="hearts-explosion">
          <span v-for="n in 18" :key="n" class="heart" :style="heartStyle(n)">💖</span>
        </div>
        <h2>Yay! I’m the luckiest 💘</h2>
        <p>Thank you for being my Valentine. You mean the world to me!</p>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const accepted = ref(false)
const noBtn = ref(null)
const noMoved = ref(false)
const noPos = ref({ x: 0, y: 0 })

function accept() {
  accepted.value = true
  setTimeout(() => window.scrollTo({ top: 0, behavior: 'smooth' }), 200)
}

function moveNo() {
  noMoved.value = true
  const w = window.innerWidth - 100
  const h = window.innerHeight - 60
  noPos.value.x = Math.random() * w
  noPos.value.y = Math.random() * h
}

function heartStyle(n) {
  const angle = (2 * Math.PI * n) / 18
  const dist = 80 + Math.random() * 40
  return {
    transform: `translate(${Math.cos(angle) * dist}px, ${Math.sin(angle) * dist}px) scale(${0.8 + Math.random() * 0.6})`,
    animationDelay: `${n * 0.07}s`
  }
}
</script>

<style scoped>
.card {
  background: #fff0f6;
  border-radius: 24px;
  box-shadow: 0 8px 32px rgba(214,51,108,0.10);
  padding: 2.5rem 1.5rem 2rem;
  text-align: center;
  max-width: 370px;
  margin: 2rem auto;
  position: relative;
  z-index: 2;
}
h1 {
  color: #d6336c;
  font-family: 'Georgia', serif;
  font-size: 2.1rem;
  margin-bottom: 1.1rem;
  font-weight: 400;
}
.message {
  color: #a61e4d;
  font-size: 1.1rem;
  margin-bottom: 2.2rem;
  font-family: 'Georgia', serif;
}
.buttons {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
}
.yes, .no {
  font-family: 'Georgia', serif;
  font-size: 1.1rem;
  padding: 0.8em 2.2em;
  border: none;
  border-radius: 30px;
  cursor: pointer;
  transition: transform 0.18s, background 0.18s;
  box-shadow: 0 2px 8px rgba(214,51,108,0.08);
}
.yes {
  background: linear-gradient(90deg, #ffb3c6 0%, #ff6f91 100%);
  color: #fff;
}
.yes:hover {
  background: #ff6f91;
  transform: scale(1.08);
}
.no {
  background: #ffe5ec;
  color: #d6336c;
  position: relative;
}
.no:hover, .no:active {
  background: #ffd6e0;
  transform: scale(1.08) rotate(-8deg);
}
.final-message {
  margin-top: 2.5rem;
  animation: popIn 0.7s;
}
@keyframes popIn {
  0% { opacity: 0; transform: scale(0.7);}
  60% { transform: scale(1.1);}
  100% { opacity: 1; transform: scale(1);}
}
.hearts-explosion {
  position: relative;
  width: 0;
  height: 0;
  margin: 0 auto 1.2rem;
}
.heart {
  position: absolute;
  left: 0; top: 0;
  font-size: 2rem;
  opacity: 0;
  animation: heart-burst 1.2s forwards;
}
@keyframes heart-burst {
  0% { opacity: 0; transform: scale(0.2);}
  30% { opacity: 1;}
  100% { opacity: 0; }
}
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.7s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
@media (max-width: 500px) {
  .card { padding: 1.2rem 0.5rem 1.2rem; }
  h1 { font-size: 1.3rem; }
  .message { font-size: 0.98rem; }
  .yes, .no { font-size: 0.98rem; padding: 0.7em 1.2em; }
}
</style>