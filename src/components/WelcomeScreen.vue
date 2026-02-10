<script setup>
import { ref } from 'vue';
import { Lock, Heart, ArrowRight } from 'lucide-vue-next';

const emit = defineEmits(['unlocked']);

const inputDate = ref('');
const error = ref(false);
const isShaking = ref(false);

const CORRECT_DATE = '17032024'; // DDMMYYYY format

const checkDate = () => {
    // Remove qualquer caractere que não seja número
    const cleanDate = inputDate.value.replace(/\D/g, '');

  if (cleanDate === CORRECT_DATE) {
    emit('unlocked');
  } else {
    triggerError();
  }
};

const triggerError = () => {
  error.value = true;
  isShaking.value = true;
  setTimeout(() => {
    isShaking.value = false;
    error.value = false;
    inputDate.value = '';
  }, 500);
};

</script>

<template>
  <div class="welcome-screen">
    <div class="content text-center">
      <div class="icon-wrapper">
        <Lock v-if="!error" :size="48" class="lock-icon" />
        <Heart v-else :size="48" class="lock-icon error-heart" />
      </div>
      
      <h1 class="welcome-title">Bem-vinda, meu amor</h1>
      <p class="welcome-subtitle">Para entrar, responda: <br> Qual a data do nosso início? (DDMMAAAA)</p>
      
      <div class="input-wrapper" :class="{ shake: isShaking }">
        <input 
          type="tel" 
          v-model="inputDate" 
          placeholder="********" 
          maxlength="8"
          class="date-input"
          @keyup.enter="checkDate"
        />
        <button class="enter-btn" @click="checkDate">
            <ArrowRight :size="24" />
        </button>
      </div>

      <p v-if="error" class="error-msg">Ops, tente de novo! ❤️</p>
    </div>
  </div>
</template>

<style scoped>
.welcome-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: linear-gradient(135deg, #FAF9F6 0%, #F5E6E3 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.content {
  padding: 2rem;
  max-width: 400px;
  width: 90%;
}

.icon-wrapper {
  margin-bottom: 2rem;
  color: var(--color-primary);
  animation: float 3s ease-in-out infinite;
}

.welcome-title {
  font-family: var(--font-heading);
  font-size: 2rem;
  color: var(--color-text);
  margin-bottom: 0.5rem;
}

.welcome-subtitle {
  font-size: 1rem;
  color: var(--color-text-light);
  margin-bottom: 2rem;
  line-height: 1.5;
}

.input-wrapper {
  display: flex;
  background: white;
  padding: 5px;
  border-radius: 50px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.05);
  border: 1px solid transparent;
  transition: all 0.3s;
}

.input-wrapper:focus-within {
    border-color: var(--color-primary);
    box-shadow: 0 4px 20px rgba(212, 165, 154, 0.2);
}

.date-input {
  flex: 1;
  border: none;
  background: transparent;
  padding: 10px 20px;
  font-size: 1.2rem;
  color: var(--color-text);
  outline: none;
  text-align: center;
  letter-spacing: 2px;
  font-family: monospace;
}

.enter-btn {
  background: var(--color-primary);
  color: white;
  border: none;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: transform 0.2s, background-color 0.2s;
}

.enter-btn:hover {
    background-color: var(--color-accent);
    transform: scale(1.05);
}

.error-msg {
    color: #e74c3c;
    margin-top: 1rem;
    font-size: 0.9rem;
    animation: fadeIn 0.3s;
}

.error-heart {
    color: #e74c3c;
}

/* Animations */
@keyframes float {
  0% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
  100% { transform: translateY(0); }
}

.shake {
  animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
}

@keyframes shake {
  10%, 90% { transform: translate3d(-1px, 0, 0); }
  20%, 80% { transform: translate3d(2px, 0, 0); }
  30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
  40%, 60% { transform: translate3d(4px, 0, 0); }
}

@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}
</style>
