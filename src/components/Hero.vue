<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { Heart, ChevronDown } from 'lucide-vue-next';

// Data de início do namoro/relacionamento (ajustado para 01/10/2022 conforme Timeline)
const startDate = new Date('2024-03-17T00:00:00');
const timeElapsed = ref({
  years: 0,
  months: 0,
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0
});

const calculateTime = () => {
  const now = new Date();
  
  let years = now.getFullYear() - startDate.getFullYear();
  let months = now.getMonth() - startDate.getMonth();
  let days = now.getDate() - startDate.getDate();
  let hours = now.getHours() - startDate.getHours();
  let minutes = now.getMinutes() - startDate.getMinutes();
  let seconds = now.getSeconds() - startDate.getSeconds();

  // Ajustes para valores negativos
  if (seconds < 0) {
    seconds += 60;
    minutes--;
  }
  if (minutes < 0) {
    minutes += 60;
    hours--;
  }
  if (hours < 0) {
    hours += 24;
    days--;
  }
  if (days < 0) {
    // Dias no mês anterior
    const prevMonth = new Date(now.getFullYear(), now.getMonth(), 0);
    days += prevMonth.getDate();
    months--;
  }
  if (months < 0) {
    months += 12;
    years--;
  }

  timeElapsed.value = { years, months, days, hours, minutes, seconds };
};

let timerInterval;

onMounted(() => {
  calculateTime();
  timerInterval = setInterval(calculateTime, 1000);
});

onUnmounted(() => {
  clearInterval(timerInterval);
});

const scrollToContent = () => {
    window.scrollBy({ top: window.innerHeight, behavior: 'smooth' });
};
</script>

<template>
  <section class="hero-section">
    <div class="hero-content text-center">
      <div class="animation-container">
        <Heart class="hero-icon" :size="64" fill="currentColor" />
      </div>
      
      <h1 class="hero-title">Bruna & Maicon</h1>
      <p class="hero-subtitle">Escrevendo nossa história juntos há</p>
      
      <div class="counter-container">
        <div class="counter-row">
          <div class="time-block">
            <span class="time-value">{{ timeElapsed.years }}</span>
            <span class="time-label">Anos</span>
          </div>
          <div class="time-block">
            <span class="time-value">{{ timeElapsed.months }}</span>
            <span class="time-label">Meses</span>
          </div>
          <div class="time-block">
            <span class="time-value">{{ timeElapsed.days }}</span>
            <span class="time-label">Dias</span>
          </div>
        </div>
      </div>

    </div>

    <button class="scroll-btn" @click="scrollToContent" aria-label="Rolar para baixo">
      <ChevronDown :size="32" />
    </button>
  </section>
</template>

<style scoped>
.hero-section {
  height: 100vh; /* Tela cheia para impacto */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: transparent;
  position: relative;
  overflow: hidden;
}

.hero-content {
  z-index: 10;
  padding: var(--spacing-md);
  margin-top: -60px; /* Ajuste visual para centralizar com a seta */
}

.hero-icon {
  color: var(--color-primary);
  margin-bottom: var(--spacing-sm);
  animation: heartbeat 2s infinite ease-in-out;
  filter: drop-shadow(0 0 10px rgba(212, 165, 154, 0.4));
}

.hero-title {
  font-family: var(--font-heading);
  font-size: 3rem; 
  font-weight: 700;
  color: #333; /* Contraste alto com fundo claro das partículas */
  margin-bottom: var(--spacing-xs);
  letter-spacing: -1px;
}

.hero-subtitle {
  font-family: var(--font-body);
  font-size: 1.1rem;
  color: #666;
  margin-bottom: var(--spacing-lg);
  font-weight: 300;
  letter-spacing: 2px;
  text-transform: uppercase;
}

/* Counter Styles */
.counter-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px; /* Espaço vertical entre as linhas */
}

.counter-row {
  display: flex;
  gap: 20px;
  justify-content: center;
  flex-wrap: wrap;
}


/* Ajuste específico para a linha de tempo no mobile se necessário */
.time-row .time-block {
  min-width: 60px;
  padding: 8px;
}

/* Ajustando valores para diferenciar hierarquia */
.time-row .time-value {
  font-size: 2rem; /* Um pouco menor que Anos/Meses */
  color: var(--color-text); /* Cor um pouco mais neutra ou manter primary */
}

.time-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 70px;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(4px);
  padding: 10px;
  border-radius: 12px;
  border: 1px solid rgba(255,255,255,0.2);
}

.time-value {
  font-family: var(--font-heading);
  font-size: 2.5rem;
  font-weight: 700;
  color: var(--color-primary);
  line-height: 1;
}

.time-label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: #555;
  margin-top: 5px;
}

.scroll-btn {
  position: absolute;
  bottom: 40px;
  background: none;
  border: none;
  color: var(--color-text-light);
  cursor: pointer;
  animation: bounce 2s infinite;
  opacity: 0.7;
  transition: opacity 0.3s;
}

.scroll-btn:hover {
  opacity: 1;
  color: var(--color-primary);
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
  40% { transform: translateY(-10px); }
  60% { transform: translateY(-5px); }
}

@media (min-width: 768px) {
  .hero-title { font-size: 4.5rem; } 
  .hero-subtitle { font-size: 1.25rem; }
  .counter-grid { gap: 30px; }
  .time-value { font-size: 3.5rem; }
}

@media (max-width: 480px) {
  .hero-title { font-size: 2.5rem; }
  .time-block { min-width: 60px; padding: 8px; }
  .time-value { font-size: 1.8rem; }
  /* Ocultar segundos/horas no mobile se ficar muito cheio, ou ajustar tamanho */
  /* .mobile-hide { display: none; } */ 
}
</style>
