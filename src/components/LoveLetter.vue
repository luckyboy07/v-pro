<template>
  <div class="container">
    <div class="envlope-wrapper">
      <div id="envelope" :class="{ close: isClosed, open: !isClosed }">
        <div class="front flap"></div>
        <div class="front pocket"></div>
        <div class="letter">
          <div class="words line1">To: Nieva Marie Tariao</div>
          <div class="words line2">Would you be my date on</div>
          <div class="words line3">Valentine's Day?</div>
          <div class="words line4">Feb. 14, 2028</div>
        </div>
        <div class="hearts">
          <div class="heart a1"></div>
          <div class="heart a2"></div>
          <div class="heart a3"></div>
        </div>
      </div>
    </div>
    <div class="reset" v-if="!isClosed">
      <div class="button-container">
        <button class="yes-button" @click="handleYes">Yes! 💖</button>
        <button 
          class="no-button" 
          @click="handleNo"
          @mouseenter="moveNoButton"
          :style="noButtonStyle"
        >
          {{ noButtonText }}
        </button>
      </div>
      <div class="attempt-message" v-if="noAttempts > 0">
        {{ getNoMessage() }}
      </div>
    </div>
    <div class="reset" v-else>
      <button @click="openEnvelope">Open</button>
    </div>

    <!-- Success Modal -->
    <div class="modal" v-if="showSuccessModal">
      <div class="modal-content">
        <div class="celebration">🎉 💖 🎊</div>
        <h2>Yes! 💕</h2>
        <p>You've made me the happiest person! ✨</p>
        <p class="date-reminder">See you on Feb. 14, 2028!</p>
        <div class="floating-hearts">
          <span class="float-heart">💖</span>
          <span class="float-heart">💕</span>
          <span class="float-heart">💗</span>
          <span class="float-heart">💓</span>
          <span class="float-heart">💝</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const isClosed = ref(true);
const noAttempts = ref(0);
const noButtonPosition = ref({ x: 0, y: 0 });
const showSuccessModal = ref(false);

const emit = defineEmits(['yes-clicked']);

const noButtonText = computed(() => {
  if (noAttempts.value === 0) return 'No';
  if (noAttempts.value < 3) return 'No...?';
  if (noAttempts.value < 5) return 'Maybe...?';
  if (noAttempts.value < 8) return 'Are you sure?';
  return 'Please? 🥺';
});

const noButtonStyle = computed<Record<string, string>>(() => {
  if (noAttempts.value === 0) return {
    position: '',
    left: '',
    top: '',
    transition: ''
  };
  return {
    position: 'absolute' as const,
    left: `${noButtonPosition.value.x}px`,
    top: `${noButtonPosition.value.y}px`,
    transition: 'all 0.3s ease'
  };
});

const openEnvelope = () => {
  isClosed.value = false;
};

const closeEnvelope = () => {
  isClosed.value = true;
  noAttempts.value = 0;
  noButtonPosition.value = { x: 0, y: 0 };
};

const moveNoButton = () => {
  noAttempts.value++;
  
  // Random position within a reasonable area below the envelope
  const maxX = 150; // horizontal movement range
  const maxY = 40; // vertical movement range
  
  noButtonPosition.value = {
    x: Math.random() * maxX - 75,
    y: Math.random() * maxY
  };
};

const handleNo = () => {
  // Make it increasingly difficult - button moves on click too
  moveNoButton();
};

const handleYes = () => {
  showSuccessModal.value = true;
  emit('yes-clicked');
  setTimeout(() => {
    showSuccessModal.value = false;
    closeEnvelope();
  }, 3000);
};

const getNoMessage = () => {
  if (noAttempts.value === 1) return "Come on, give it a chance! 💫";
  if (noAttempts.value === 2) return "Really? Think about it... 💭";
  if (noAttempts.value === 3) return "The 'Yes' button is right there! 👉";
  if (noAttempts.value === 4) return "I promise it'll be fun! 🎈";
  if (noAttempts.value === 5) return "Just one chance? 🙏";
  if (noAttempts.value === 6) return "You're making this hard... 😅";
  if (noAttempts.value === 7) return "Pretty please? 🥺";
  return "I believe you'll say yes eventually! 💪";
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400..700&display=swap');

.container {
  min-height: 100vh;
  background: linear-gradient(135deg, #ffeef8 0%, #ffe0f0 100%);
  padding: 20px;
}

#envelope {
  position: relative;
  width: 280px;
  height: 180px;
  border-bottom-left-radius: 6px;
  border-bottom-right-radius: 6px;
  margin-left: auto;
  margin-right: auto;
  top: 150px;
  background-color: #d9534f;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.front {
  position: absolute;
  width: 0;
  height: 0;
  z-index: 3;
}

.flap {
  border-left: 140px solid transparent;
  border-right: 140px solid transparent;
  border-bottom: 82px solid transparent;
  border-top: 98px solid #d9534f;
  transform-origin: top;
  pointer-events: none;
}

.pocket {
  border-left: 140px solid #df716f;
  border-right: 140px solid #df716f;
  border-bottom: 90px solid #e44b3c;
  border-top: 90px solid transparent;
  border-bottom-left-radius: 6px;
  border-bottom-right-radius: 6px;
}

.letter {
  position: relative;
  background-color: #fff;
  width: 90%;
  margin-left: auto;
  margin-right: auto;
  height: 90%;
  top: 5%;
  border-radius: 6px;
  box-shadow: 0 2px 26px rgba(0, 0, 0, 0.12);
  font-family: "Dancing Script", cursive;
}

.letter:after {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.words {
  position: absolute;
  left: 10%;
  width: 80%;
  height: 14%;
  background-color: transparent;
  color: #000;
}

.words.line1 {
  top: 10%;
  width: 60%;
  height: 7%;
  font-size: 1rem;
  font-weight: 600;
}

.words.line2 {
  top: 30%;
  text-align: center;
  font-size: 1.1rem;
}

.words.line3 {
  top: 50%;
  font-size: 1.1rem;
  text-align: center;
}

.words.line4 {
  top: 70%;
  font-size: 1.1rem;
  text-align: center;
}

.button-container {
  position: relative;
  display: flex;
  gap: 15px;
  margin-top: 20px;
  min-height: 80px;
  width: 100%;
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
  justify-content: center;
  align-items: flex-start;
}

.yes-button,
.no-button {
  font-family: "Dancing Script", cursive;
  font-weight: 700;
  font-size: 1.1rem;
  padding: 12px 24px;
  border-radius: 25px;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.yes-button {
  background: linear-gradient(135deg, #ff6b9d 0%, #d9534f 100%);
  color: white;
  transform: scale(1);
}

.yes-button:hover {
  transform: scale(1.1);
  box-shadow: 0 6px 20px rgba(217, 83, 79, 0.4);
}

.no-button {
  background: #f0f0f0;
  color: #666;
  z-index: 10;
}

.no-button:hover {
  background: #e0e0e0;
}

.attempt-message {
  margin-top: 10px;
  font-size: 1rem;
  color: #d9534f;
  text-align: center;
  font-style: italic;
  font-family: "Dancing Script", cursive;
  font-weight: 600;
  min-height: 25px;
}

.open .flap {
  transform: rotateX(180deg);
  transition: transform 0.4s ease, z-index 0.6s;
  z-index: 1;
}

.close .flap {
  transform: rotateX(0deg);
  transition: transform 0.4s 0.6s ease, z-index 1s;
  z-index: 5;
}

.close .letter {
  transform: translateY(0px);
  transition: transform 0.4s ease, z-index 1s;
  z-index: 1;
}

.open .letter {
  transform: translateY(-100px);
  transition: transform 0.4s 0.6s ease, z-index 0.6s;
  z-index: 2;
}

.hearts {
  position: absolute;
  top: 90px;
  left: 0;
  right: 0;
  z-index: 2;
}

.heart {
  position: absolute;
  bottom: 0;
  right: 10%;
  pointer-events: none;
}

.heart:before,
.heart:after {
  position: absolute;
  content: "";
  left: 50px;
  top: 0;
  width: 50px;
  height: 80px;
  background: #e60073;
  border-radius: 50px 50px 0 0;
  transform: rotate(-45deg);
  transform-origin: 0 100%;
  pointer-events: none;
}

.heart:after {
  left: 0;
  transform: rotate(45deg);
  transform-origin: 100% 100%;
}

.close .heart {
  opacity: 0;
  animation: none;
}

.a1 {
  left: 20%;
  transform: scale(0.6);
  opacity: 1;
  animation: slideUp 4s linear 1, sideSway 2s ease-in-out 4 alternate;
  animation-fill-mode: forwards;
  animation-delay: 0.7s;
}

.a2 {
  left: 55%;
  transform: scale(1);
  opacity: 1;
  animation: slideUp 5s linear 1, sideSway 4s ease-in-out 2 alternate;
  animation-fill-mode: forwards;
  animation-delay: 0.7s;
}

.a3 {
  left: 10%;
  transform: scale(0.8);
  opacity: 1;
  animation: slideUp 7s linear 1, sideSway 2s ease-in-out 6 alternate;
  animation-fill-mode: forwards;
  animation-delay: 0.7s;
}

@keyframes slideUp {
  0% {
    top: 0;
  }
  100% {
    top: -600px;
  }
}

@keyframes sideSway {
  0% {
    margin-left: 0px;
  }
  100% {
    margin-left: 50px;
  }
}

.envlope-wrapper {
  height: 380px;
}

.reset {
  text-align: center;
}

.reset button {
  font-weight: 800;
  font-style: normal;
  transition: all 0.1s linear;
  background-color: transparent;
  border: solid 2px #d9534f;
  border-radius: 4px;
  color: #000000;
  display: inline-block;
  font-size: 14px;
  text-transform: uppercase;
  margin: 5px;
  padding: 10px;
  line-height: 1em;
  text-decoration: none;
  min-width: 120px;
  cursor: pointer;
}

.reset button:hover {
  background-color: #d9534f;
  color: #fff;
}

/* Success Modal Styles */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  animation: fadeIn 0.3s ease;
}

.modal-content {
  background: white;
  padding: 40px;
  border-radius: 20px;
  text-align: center;
  max-width: 400px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
  animation: scaleIn 0.4s ease;
}

.celebration {
  font-size: 3rem;
  margin-bottom: 20px;
  animation: bounce 1s ease infinite;
}

.modal-content h2 {
  color: #d9534f;
  font-family: "Dancing Script", cursive;
  font-size: 2rem;
  margin-bottom: 15px;
}

.modal-content p {
  font-size: 1.1rem;
  color: #666;
  margin: 10px 0;
}

.date-reminder {
  font-weight: 600;
  color: #d9534f;
  margin-top: 20px;
  font-size: 1.2rem;
}

.floating-hearts {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 10px;
}

.float-heart {
  font-size: 1.5rem;
  animation: floatUp 2s ease-in-out infinite;
  display: inline-block;
}

.float-heart:nth-child(1) { animation-delay: 0s; }
.float-heart:nth-child(2) { animation-delay: 0.2s; }
.float-heart:nth-child(3) { animation-delay: 0.4s; }
.float-heart:nth-child(4) { animation-delay: 0.6s; }
.float-heart:nth-child(5) { animation-delay: 0.8s; }

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes scaleIn {
  from { transform: scale(0.7); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

@keyframes floatUp {
  0%, 100% { transform: translateY(0); opacity: 1; }
  50% { transform: translateY(-15px); opacity: 0.7; }
}
</style>