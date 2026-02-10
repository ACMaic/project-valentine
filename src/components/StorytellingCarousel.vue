<script setup>
import { ref, computed } from 'vue';
import { useMotion } from '@vueuse/motion';
import { Heart, Star, MapPin, Calendar, Smile, Music, Sun, BookOpen } from 'lucide-vue-next';

// Importando imagens locais
import img1 from '../assets/foto-1.jpg';
import img2 from '../assets/foto-2.jpg';
import img3 from '../assets/foto-3.jpg';
import img4 from '../assets/foto-4.jpg';
import img5 from '../assets/foto-5.jpg';
import img6 from '../assets/foto-6.jpg';
import img7 from '../assets/foto-7.jpg';

const stories = ref([
  {
    id: 1,
    image: img1, 
    caption: "Nosso Primeiro Encontro",
    fact: "Taça Cheia Bar, Osasco",
    description: "Onde tudo começou. Um brinde ao nosso destino!",
    color: "#ffecd2"
  },
  {
    id: 2,
    image: img2,
    caption: "Nossa Primeira Foto",
    fact: "Desde o inicio a dieta indo de base rs",
    description: "A primeira de muitas fotos juntos!    Aoooo Michele .",
    color: "#fcb69f"
  },
  {
    id: 3,
    image: img3,
    caption: "Nas Redes 👀",
    fact: "Primeira foto juntos oficial, claro que não foi essa, mas foi nesse dia rs",
    description: "Compartilhando nossa felicidade com o mundo, infelicidade de alguns rs.",
    color: "#a18cd1"
  },
  {
    id: 4,
    image: img4,
    caption: "Na Arena",
    fact: "Arena Corinthians",
    description: "Vibrando juntos na mesma sintonia.",
    color: "#ffecd2"
  },
  {
    id: 5,
    image: img5,
    caption: "Vitamina D",
    fact: "Nossas fugas para o mar",
    description: "Sol, areia e a melhor companhia do mundo.",
    color: "#E5D3B3"
  },
  {
    id: 6,
    image: img6,
    caption: "Nossa Batida",
    fact: "Shows e Festas Inesquecíveis",
    description: "Dançando juntos até o amanhecer, CONTEM IRONIA.",
    color: "#D4A59A"
  },
  {
    id: 7,
    image: img7,
    caption: "O Próximo Capítulo",
    fact: "O melhor ainda está por vir",
    description: "Construindo nossos sonhos, um dia de cada vez.",
    color: "#D4AF37"
  }
]);

const currentIndex = ref(0);

// Função para avançar
const nextCard = () => {
    if (currentIndex.value < stories.value.length - 1) {
        currentIndex.value++;
    } else {
        // Loop para o início
        currentIndex.value = 0;
    }
};

// Computa estilos dinâmicos para cada carta com base no índice atual
const getCardStyle = (index) => {
    const delta = index - currentIndex.value;

    // Cartas passadas (saem para a esquerda com rotação)
    if (delta < 0) {
        return {
            transform: `translateX(-150%) rotate(-20deg) scale(0.8)`,
            opacity: 0,
            zIndex: 0,
            pointerEvents: 'none'
        };
    }

    // Carta atual
    if (delta === 0) {
        return {
            transform: `translateX(0) rotate(0) scale(1)`,
            opacity: 1,
            zIndex: stories.value.length,
            boxShadow: '0 25px 50px -12px rgba(0,0,0,0.25)' // Sombra mais suave e moderna
        };
    }

    // Cartas futuras (empilhadas atrás)
    if (delta > 0) {
        const scale = 1 - (delta * 0.05); // Diminui escala
        const translateY = delta * 15; // Desce um pouco
        
        // Limitamos a visibilidade a 3 cartas futuras para performance
        const opacity = delta > 2 ? 0 : 1;

        return {
            transform: `translateY(${translateY}px) scale(${scale})`,
            zIndex: stories.value.length - delta,
            opacity: opacity
        };
    }
};

</script>

<template>
  <section class="storytelling-section" v-motion-slide-visible-once-bottom :duration="800">
    <h2 class="section-title text-center">Nossa História em Capítulos</h2>
    
    <div class="carousel-stage">
      <div 
        v-for="(story, index) in stories" 
        :key="story.id"
        class="card-container"
        :style="getCardStyle(index)"
        @click="index === currentIndex ? nextCard() : null"
      >
        <div class="card-inner glass-effect">
            <div class="photo-frame">
                <img :src="story.image" :alt="story.caption" draggable="false" />
            </div>
            
            <div class="card-content">
                <h3 class="card-title">{{ story.caption }}</h3>
                <p class="card-desc">{{ story.description }}</p>
                
                <div class="card-fact-box" v-if="index === currentIndex" v-motion-fade-visible>
                    <span class="fact-icon"><Star :size="14" /> {{ story.id === 1 ? 'Local:' : 'Curiosidade:' }}</span>
                    <p class="fact-text">{{ story.fact }}</p>
                </div>
            </div>

            <div class="card-footer" v-if="index === currentIndex">
               <span class="tap-hint">Toque para continuar <Heart :size="12" fill="#D4A59A" /></span> 
            </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.storytelling-section {
    padding: var(--spacing-lg) 0;
    min-height: 700px;
    padding: var(--spacing-lg) 0;
    min-height: 700px;
    background: transparent; 
    overflow: hidden; 
    overflow: hidden;
    position: relative;
}

.section-title {
  font-size: 2rem;
  margin-bottom: var(--spacing-lg);
  color: var(--color-primary);
  text-shadow: 0 2px 10px rgba(0,0,0,0.05);
}

.carousel-stage {
    position: relative;
    width: 320px;
    max-width: 90%;
    height: 550px;
    margin: 0 auto;
    perspective: 1000px;
}

.card-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transition: all 0.6s cubic-bezier(0.25, 0.8, 0.25, 1);
    transform-origin: center bottom;
    cursor: pointer;
    display: flex;
    justify-content: center;
}

/* Glassmorphism Effect */
.glass-effect {
    background: rgba(255, 255, 255, 0.75);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border: 1px solid rgba(255, 255, 255, 0.5);
    box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.1);
}

.card-inner {
    width: 100%;
    height: 100%;
    padding: 24px;
    border-radius: 24px;
    display: flex;
    flex-direction: column;
}

.photo-frame {
    width: 100%;
    height: 280px;
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 20px;
    background-color: #f0f0f0;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}

.photo-frame img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease;
}

.card-container:hover .photo-frame img {
    transform: scale(1.05);
}

.card-content {
    flex: 1;
    text-align: center;
    display: flex;
    flex-direction: column;
}

.card-title {
    font-family: var(--font-heading);
    font-size: 1.6rem;
    color: var(--color-text);
    margin-bottom: 8px;
    letter-spacing: -0.5px;
}

.card-desc {
    font-size: 1rem;
    color: var(--color-text-light);
    margin-bottom: 16px;
    line-height: 1.5;
}

.card-fact-box {
    background-color: rgba(212, 165, 154, 0.15);
    padding: 14px;
    border-radius: 12px;
    border-left: 4px solid var(--color-primary);
    text-align: left;
    margin-top: auto;
}

.fact-icon {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.75rem;
    text-transform: uppercase;
    color: var(--color-primary);
    font-weight: 700;
    margin-bottom: 4px;
}

.fact-text {
    font-size: 0.95rem;
    color: var(--color-text);
    font-style: italic;
    font-weight: 500;
}

.card-footer {
    margin-top: 15px;
    text-align: center;
}

.tap-hint {
    font-size: 0.85rem;
    color: var(--color-text-light);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 6px;
    opacity: 0.8;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

/* Responsividade para iPhone 12 Pro e menores */
@media (max-width: 400px) {
    .storytelling-section {
        padding: var(--spacing-md) 0;
        min-height: 650px;
    }
    
    .section-title {
        font-size: 1.75rem;
        margin-bottom: var(--spacing-md);
    }
    
    .carousel-stage {
        width: 100%;
        padding: 0 20px;
        height: 520px;
    }
    
    .card-inner {
        padding: 20px;
    }
    
    .photo-frame {
        height: 240px;
    }
    
    .card-title {
        font-size: 1.4rem;
    }
    
    .card-desc {
        font-size: 0.9rem;
    }
}
</style>
