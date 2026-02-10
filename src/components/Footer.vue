<script setup>
import { Heart } from 'lucide-vue-next';
</script>

<template>
  <footer class="footer">
    <div class="love-letter-container">
      <h2 class="letter-title">
        Para a mulher que mudou minha vida, <span class="highlight">Bruna</span>!<br>
        Com todo o meu amor, <span class="highlight">Maicon</span>.
      </h2>
      
      <p class="letter-body">
        Cada um desses 2 anos foi um capítulo incrível. Obrigado por iluminar meus dias, 
        por ser minha parceira, minha melhor amiga e o amor da minha vida. 
        Você é a melhor parte da minha história.
      </p>

      <p class="letter-signature">
        Feliz 2 anos, Amor! 
        <Heart class="heart-icon-inline" :size="24" fill="currentColor" />
      </p>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  text-align: center;
  padding: 6rem 1.5rem;
  background-color: transparent; /* Particles visible behind */
  color: #F5F5F5; /* Off-white base */
  position: relative;
  overflow: hidden;
}

.love-letter-container {
  max-width: 800px;
  margin: 0 auto;
  /* Dark Glassmorphism */
  background: rgba(18, 18, 18, 0.8);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  
  padding: 4rem 3rem; /* Generous padding */
  border-radius: 24px;
  
  /* Subtle Dry Rose Border */
  border: 1px solid rgba(212, 165, 154, 0.3);
  
  box-shadow: 0 20px 50px rgba(0,0,0,0.5);
  transition: transform 0.3s ease, border-color 0.3s ease;
}

.love-letter-container:hover {
  border-color: rgba(212, 165, 154, 0.5);
  transform: translateY(-5px);
}

.letter-title {
  font-family: 'Playfair Display', serif; /* Elegant Serif */
  font-size: 2.2rem;
  font-weight: 700;
  line-height: 1.3;
  margin-bottom: 2rem;
  color: #F5F5F5; /* Off-white */
}

.highlight {
  color: #D4A59A; /* Rosa Seco */
  font-style: italic;
  font-weight: 600; /* Slightly bolder for names */
}

.letter-body {
  font-family: 'Playfair Display', serif;
  font-size: 1.25rem;
  line-height: 1.9;
  color: #F5F5F5; /* Off-white */
  margin-bottom: 3rem;
  font-weight: 400;
  opacity: 0.9;
}

.letter-signature {
  font-family: 'Playfair Display', serif;
  font-size: 1.8rem; /* Slightly larger */
  font-weight: 700;
  color: #D4A59A; /* Rosa Seco for "Feliz 2 anos" */
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  margin-top: 1rem;
}

.heart-icon-inline {
  color: #D4A59A;
  fill: #D4A59A; /* Filled as requested */
  animation: heartbeat 2s infinite ease-in-out;
  /* Glow/Box Shadow Effect on the icon itself (using filter drop-shadow for SVG) */
  filter: drop-shadow(0 0 8px rgba(212, 165, 154, 0.6));
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); filter: drop-shadow(0 0 8px rgba(212, 165, 154, 0.6)); }
  50% { transform: scale(1.15); filter: drop-shadow(0 0 15px rgba(212, 165, 154, 0.9)); }
}

@media (max-width: 768px) {
  .footer { padding: 4rem 1rem; }
  .love-letter-container { padding: 2.5rem 1.5rem; }
  .letter-title { font-size: 1.75rem; }
  .letter-body { font-size: 1.1rem; line-height: 1.6; }
  .letter-signature { font-size: 1.5rem; }
}
</style>
