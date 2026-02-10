<script setup>
import { ref } from 'vue';
import { 
  Calendar, MapPin, Smile, Heart, Music, Star, Camera, Sun, 
  Sparkles, Bike, Waves 
} from 'lucide-vue-next';

// Reatividade para o item ativo (aberto)
const activeMilestone = ref(null);

// Lista de Dados: 'milestones' conforme solicitado
const milestones = ref([
  {
    id: 1,
    date: '30 Setembro 2022',
    title: 'Um Dia Especial',
    description: 'Provavelmente você não sentiu o que eu senti, mas, desde aquele primeiro contato, meu interesse por você já foi muito além. A conversa fluiu tanto que, naquele dia, eu já tinha certeza de que queria você ao meu lado.',
    icon: Sparkles, 
    color: '#F5E6CC' 
  },
  {
    id: 2,
    date: '01 Outubro 2022',
    title: 'Nosso Primeiro Encontro',
    description: 'Nosso primeiro encontro oficial. Não havia nada que pudesse nos impedir, a não ser o meu nervosismo. Eu mal conseguia acreditar no que estava acontecendo; depois de tanto tempo, eu tinha você ali, na minha frente, e nem sabia por onde começar, tamanha era a minha vontade de estar contigo.',
    icon: Music,
    color: '#D4A59A' 
  },
  {
    id: 3,
    date: '03 Novembro 2023', 
    title: 'A Primeira Viagem',
    description: '"Sabor" viagem, né? Foi bom demais ter a experiência de pegar a estrada com você na garupa. Itu ficou pequena para o tamanho do nosso amor! Que venham muitos outros quilômetros ao seu lado.',
    icon: Bike,
    color: '#E4BCB2' 
  },
  {
    id: 4,
    date: '17 Março 2024',
    title: 'O Pedido',
    description: 'Diante daquele lugar lindo onde estávamos e no cenário que você mais ama, eu não podia perder a oportunidade. Eu já vinha planejando, mas aquele sol e aquele mar foram os cúmplices perfeitos para oficializar nosso pedido de namoro. Mesmo sem aliança naquele momento, o meu "sim" já era todo seu. 🎶 "Quero ser feliz também, navegar nas ondas do seu mar..."',
    icon: Waves, 
    color: '#B2BABB' 
  },
  {
    id: 5,
    date: 'Hoje',
    title: 'Para Sempre',
    description: 'O início de todos os nossos próximos anos. Que nossa história continue sendo escrita com muito amor.',
    icon: Star,
    color: '#D4AF37'
  }
]);

const toggleMilestone = (index) => {
    if (activeMilestone.value === index) {
        activeMilestone.value = null;
    } else {
        activeMilestone.value = index;
    }
};

// Lógica de Efeito 3D (Tilt + Parallax)
const cardTransforms = ref(milestones.value.map(() => ({ rotateX: 0, rotateY: 0, scale: 1, dotX: 0, dotY: 0 })));

const handleMouseMove = (e, index) => {
    // Apenas Desktop
    if (window.innerWidth < 768) return;

    const card = e.currentTarget;
    const rect = card.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    
    // Centro do card
    const centerX = rect.width / 2;
    const centerY = rect.height / 2;
    
    // Rotação (Invertendo Y para tilt natural)
    const rotateX = ((y - centerY) / centerY) * -8; 
    const rotateY = ((x - centerX) / centerX) * 8;

    // Parallax do ponto (movimento oposto)
    const dotX = ((x - centerX) / centerX) * -6;
    const dotY = ((y - centerY) / centerY) * -6;

    cardTransforms.value[index] = {
        rotateX,
        rotateY,
        scale: 1.02, // Leve aumento
        dotX,
        dotY
    };
};

const handleMouseLeave = (index) => {
    // Resetar transformação
    cardTransforms.value[index] = { rotateX: 0, rotateY: 0, scale: 1, dotX: 0, dotY: 0 };
};

</script>

<template>
  <section class="timeline-section container">
    <h2 class="section-title text-center">Nossa Jornada</h2>
    
    <div class="timeline-container">
        <!-- Linha Central -->
        <div class="timeline-line"></div>

        <div 
            v-for="(milestone, index) in milestones" 
            :key="milestone.id" 
            class="timeline-item"
            :class="{ 'active': activeMilestone === index }"
            @click="toggleMilestone(index)"
            @mousemove="(e) => handleMouseMove(e, index)"
            @mouseleave="() => handleMouseLeave(index)"
            :style="{
                transform: `
                    perspective(1000px) 
                    rotateX(${cardTransforms[index].rotateX}deg) 
                    rotateY(${cardTransforms[index].rotateY}deg) 
                    scale(${activeMilestone === index ? 1.05 : cardTransforms[index].scale})
                `
            }"
        >
            <!-- Lado Esquerdo (Data) -->
            <div class="timeline-side-date">
                <span class="date-text">{{ milestone.date }}</span>
            </div>

            <!-- Ponto Central (Ícone Parallax) -->
            <div class="timeline-dot-wrapper">
                <div 
                    class="timeline-dot"
                    :style="{ 
                        backgroundColor: activeMilestone === index ? milestone.color : '#FFF', 
                        borderColor: milestone.color,
                        transform: `translateX(${cardTransforms[index].dotX}px) translateY(${cardTransforms[index].dotY}px)` 
                    }"
                >
                    <component 
                        :is="milestone.icon" 
                        :size="20" 
                        :color="activeMilestone === index ? 'white' : milestone.color" 
                        class="dot-icon"
                    />
                </div>
            </div>

            <!-- Lado Direito (Card 3D + Conteúdo) -->
            <div class="timeline-content-wrapper">
                <div class="timeline-card-content">
                    <div class="timeline-head">
                        <h3 class="milestone-title">{{ milestone.title }}</h3>
                        <span class="expand-hint" v-show="activeMilestone !== index">+</span>
                    </div>
                    
                    <!-- Animação de Expansão (estilo v-expand-transition) -->
                    <transition name="sprout">
                        <div v-show="activeMilestone === index" class="milestone-details">
                            <p class="milestone-desc">{{ milestone.description }}</p>
                        </div>
                    </transition>
                </div>
            </div>
        </div>
    </div>
  </section>
</template>

<style scoped>
.timeline-section {
    padding: 4rem 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow: hidden; /* Evita que o 3D 'vaze' da tela */
}

.section-title {
  font-size: 2.5rem;
  margin-bottom: 3rem;
  color: var(--color-primary);
  font-family: 'Playfair Display', serif;
}

.timeline-container {
    position: relative;
    max-width: 800px;
    width: 100%;
    padding: 20px 0;
}

/* Linha Vertical */
.timeline-line {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    top: 0;
    bottom: 0;
    width: 2px;
    background-color: #E0E0E0;
    z-index: 0;
}

.timeline-item {
    display: flex;
    align-items: flex-start;
    margin-bottom: 2.5rem;
    position: relative;
    cursor: pointer;
    
    /* Propriedades 3D */
    transition: transform 0.1s ease-out; /* Rápido para seguir o mouse */
    transform-style: preserve-3d;
    border-radius: 16px;
}

/* Data */
.timeline-side-date {
    flex: 1;
    text-align: right;
    padding-right: 2rem;
    padding-top: 14px; 
    transform: translateZ(20px); /* Profundidade */
}

.date-text {
    font-weight: 600;
    color: var(--color-text-light);
    font-size: 0.9rem;
    text-transform: uppercase;
    letter-spacing: 1px;
}

/* Dot */
.timeline-dot-wrapper {
    width: 60px;
    display: flex;
    justify-content: center;
    position: relative;
    z-index: 2;
    transform: translateZ(50px); /* Flutuar acima */
}

.timeline-dot {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    border: 3px solid;
    display: flex;
    justify-content: center;
    align-items: center;
    justify-content: center;
    align-items: center;
    background: rgba(255, 255, 255, 0.8);
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    transition: transform 0.1s ease-out, background-color 0.3s, border-color 0.3s;
}

/* Conteúdo */
.timeline-content-wrapper {
    flex: 1;
    padding-left: 2rem;
    padding-top: 0;
    transform: translateZ(30px);
}

.timeline-card-content {
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(5px);
    padding: 1.25rem;
    border-radius: 16px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.02);
    border: 1px solid rgba(0,0,0,0.03);
    transition: box-shadow 0.3s;
}

.timeline-item:hover .timeline-card-content {
    box-shadow: 0 20px 40px rgba(212, 165, 154, 0.2); /* Glow */
}

.timeline-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.expand-hint {
    color: var(--color-primary);
    font-size: 1.2rem;
    font-weight: bold;
    opacity: 0.7;
}

.milestone-title {
    font-size: 1.15rem;
    font-weight: 600;
    color: var(--color-text);
}

.timeline-item.active .milestone-title {
    color: var(--color-primary);
}

.milestone-details {
    overflow: hidden;
    color: var(--color-text-light);
    line-height: 1.6;
    font-size: 0.95rem;
    margin-top: 0.5rem;
    border-top: 1px solid #f0f0f0;
    padding-top: 0.5rem;
}

/* Animação "Brotar" (substitui v-expand-transition) */
.sprout-enter-active {
  animation: sprout-in 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.sprout-leave-active {
  animation: sprout-in 0.3s reverse ease-in;
}

@keyframes sprout-in {
    0% {
        opacity: 0;
        transform: scaleY(0.5) translateY(-10px);
        max-height: 0;
    }
    40% {
        opacity: 0.5;
        max-height: 50px;
    }
    100% {
        opacity: 1;
        transform: scaleY(1) translateY(0);
        max-height: 300px;
    }
}

/* Mobile Reset */
@media (max-width: 768px) {
    .timeline-item {
        transform: none !important; /* Sem tilt */
        transition: transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }
    
    .timeline-item:active {
        animation: mobile-pulse 0.3s ease-in-out;
    }
    
    @keyframes mobile-pulse {
        0% { transform: scale(1); }
        50% { transform: scale(0.96); }
        100% { transform: scale(1); }
    }
    
    .timeline-dot {
        transform: none !important; /* Sem parallax */
    }

    /* Layout Adjustments (same as before) */
    .timeline-line { left: 30px; transform: none; }
    .timeline-item { flex-direction: column; padding-left: 60px; }
    .timeline-side-date { text-align: left; padding-right: 0; margin-bottom: 4px; order: -1; padding-left: 4px; transform: none; }
    .timeline-dot-wrapper { position: absolute; left: 0; top: 0; width: 60px; height: 100%; display: block; transform: none; }
    .timeline-dot { margin-left: 8px; margin-top: 8px; }
    .timeline-content-wrapper { padding-left: 4px; padding-top: 0; width: 100%; transform: none; }
}
</style>


