<script setup>
import { Sun, Heart, Coffee, Smile, Music, Star } from 'lucide-vue-next';

const reasons = [
  { icon: Sun, text: "Seu sorriso ilumina meu dia" },
  { icon: Heart, text: "Você tem o maior coração do mundo" },
  { icon: Coffee, text: "Nossos cafés da manhã, mesmo que você tenha trabalho para me tirar da cama rsrs" },  
  { icon: Smile, text: "Minhas piadas ruins (que voce ri sempre rsrs)" },
  { icon: Music, text: "Nossa playlist compartilhada" },
  { icon: Star, text: "Você me faz uma pessoa melhor" }
];
</script>

<template>
  <section class="reasons-section container">
    <h2 class="section-title text-center">Por Que Eu Te Amo</h2>
    <div class="reasons-grid">
      <div v-for="(reason, index) in reasons" :key="index" class="reason-card">
        <component :is="reason.icon" :size="32" class="reason-icon" />
        <p class="reason-text">{{ reason.text }}</p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.reasons-section {
  padding: var(--spacing-lg) 0;
  padding: var(--spacing-lg) 0;
  background-color: transparent;
}

.section-title {
  font-size: 2.5rem;
  margin-bottom: var(--spacing-lg);
  color: var(--color-primary);
}

.reasons-grid {
  display: grid;
  /* Mobile First: 1 coluna por padrão (cols="12") */
  grid-template-columns: 1fr;
  gap: var(--spacing-md);
  width: 100%;
  max-width: 1000px; /* Limite máximo para telas grandes */
  margin: 0 auto;
}

.reason-card {
  background: rgba(255, 255, 255, 0.6); /* Semi-transparent */
  backdrop-filter: blur(8px);
  padding: var(--spacing-md);
  border-radius: var(--radius-lg);
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  box-shadow: var(--shadow-sm);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border: 1px solid transparent;
  
  /* Garante que o card não estoure a tela */
  width: 100%; 
  max-width: 100%;
}

.reason-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-lg);
  border-color: var(--color-primary-light);
}

.reason-icon {
  color: var(--color-accent);
  margin-bottom: var(--spacing-sm);
  /* Garante que o ícone não estoure */
  max-width: 100%;
}

.reason-text {
  font-weight: 500;
  color: var(--color-text);
  font-size: 1rem;
  
  /* Evita texto saindo do card */
  word-break: break-word;
  overflow-wrap: break-word;
  line-height: 1.4;
}

/* Telas maiores (Tablet/Desktop) - Equivalente a md="4" (3 colunas) */
@media (min-width: 768px) {
  .reasons-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
