<template>
  <div class="app">
    <FloatingHearts ref="floatingHeartsRef"/>
    <!-- <ProposalCard /> -->
    <LoveLetter @yes-clicked="handleYesClicked"/>
    <!-- <button class="music-toggle" @click="toggleMusic">
      <span v-if="musicPlaying">🔊</span>
      <span v-else>🔈</span>
      Music
    </button>
    <audio ref="audio" loop src="https://cdn.pixabay.com/audio/2022/10/16/audio_12b6b7b7b2.mp3"></audio> -->
  </div>
</template>

<script setup>
import { ref } from 'vue'
import LoveLetter from './components/LoveLetter.vue'
import FloatingHearts from './components/FloatingHearts.vue'
import ProposalCard from './components/ProposalCard.vue'

const audio = ref(null)
const musicPlaying = ref(false)
const floatingHeartsRef = ref()

const handleYesClicked = () => {
  floatingHeartsRef.value?.switchToImages()
}

function toggleMusic() {
  if (!audio.value) return
  if (musicPlaying.value) {
    audio.value.pause()
    musicPlaying.value = false
  } else {
    audio.value.play()
    musicPlaying.value = true
  }
}
</script>

<style>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #ffe5ec 0%, #fff1f7 100%);
  position: relative;
  overflow: hidden;
}
.music-toggle {
  position: fixed;
  bottom: 24px;
  right: 24px;
  background: #fff0f6;
  border: none;
  border-radius: 24px;
  padding: 10px 18px;
  font-size: 1.1rem;
  color: #d6336c;
  box-shadow: 0 2px 8px rgba(214,51,108,0.08);
  cursor: pointer;
  z-index: 10;
  transition: background 0.2s;
}
.music-toggle:hover {
  background: #ffd6e0;
}
</style>