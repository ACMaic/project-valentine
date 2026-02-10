<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const canvasRef = ref(null);
let animationFrameId = null;
let particles = [];
let ctx = null;

const mouse = { x: null, y: null, radius: 150 };

// Paleta: Rosa Seco, Off-white, Dourado Suave
const colors = ['#D4A59A', '#F5F5F5', '#E6D2AA'];

class Particle {
    constructor(w, h) {
        this.x = Math.random() * w;
        this.y = Math.random() * h;
        this.size = Math.random() * 3 + 1; 
        this.baseX = this.x;
        this.baseY = this.y;
        this.density = (Math.random() * 30) + 1;
        this.color = colors[Math.floor(Math.random() * colors.length)];
        this.speedX = (Math.random() * 1) - 0.5;
        this.speedY = (Math.random() * 1) - 0.5;
    }

    draw() {
        if (!ctx) return;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fillStyle = this.color;
        ctx.globalAlpha = 0.6; 
        ctx.fill();
        ctx.globalAlpha = 1.0;
    }

    update() {
        // Movimento autônomo suave
        this.x += this.speedX;
        this.y += this.speedY;

        // Interação com Mouse (Desktop) e Touch (Mobile)
        if (mouse.x != null) {
            let dx = mouse.x - this.x;
            let dy = mouse.y - this.y;
            let distance = Math.sqrt(dx*dx + dy*dy);
            
            if (distance < mouse.radius) {
                const forceDirectionX = dx / distance;
                const forceDirectionY = dy / distance;
                const maxDistance = mouse.radius;
                const force = (maxDistance - distance) / maxDistance;
                const directionX = forceDirectionX * force * this.density;
                const directionY = forceDirectionY * force * this.density;
                
                // ATRAÇÃO: Partículas são atraídas pelo mouse
                this.x += directionX;
                this.y += directionY;
            }
        }

        // Interação especial no rodapé (Aura/Ninho)
        // Se a partícula estiver nos últimos 20% da tela (área do footer)
        if (this.y > window.innerHeight * 0.8) {
             // Movimento mais lento e "flutuante" (aura)
             this.x += Math.sin(this.y * 0.1) * 0.2; 
             // Leve atração para o centro X se não houver mouse
             if (mouse.x == null) {
                 const centerX = window.innerWidth / 2;
                 const distToCenter = centerX - this.x;
                 this.x += distToCenter * 0.0005; 
             }
        }

        // Rebater nas bordas
        if (this.x < 0 || this.x > window.innerWidth) this.speedX = -this.speedX;
        if (this.y < 0 || this.y > window.innerHeight) this.speedY = -this.speedY;

        this.draw();
    }
}

const init = () => {
    particles = [];
    if (!canvasRef.value) return;
    
    // Ajusta quantidade para mobile/desktop
    const count = window.innerWidth < 768 ? 50 : 120; // Mais partículas para efeito nuvem
    
    for (let i = 0; i < count; i++) {
        particles.push(new Particle(window.innerWidth, window.innerHeight));
    }
};

const animate = () => {
    if (!canvasRef.value) return;
    ctx.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height);
    
    particles.forEach(p => p.update());
    
    animationFrameId = requestAnimationFrame(animate);
};

const handleResize = () => {
    if (canvasRef.value) {
        canvasRef.value.width = window.innerWidth;
        canvasRef.value.height = window.innerHeight;
        init();
    }
};

const handleMouseMove = (e) => {
    mouse.x = e.x;
    mouse.y = e.y;
};

const handleTouchMove = (e) => {
    if (e.touches.length > 0) {
        mouse.x = e.touches[0].clientX;
        mouse.y = e.touches[0].clientY;
    }
};

const handleInteractionEnd = () => {
    mouse.x = null;
    mouse.y = null;
};

onMounted(() => {
    const canvas = canvasRef.value;
    if (canvas) {
        ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        
        init();
        animate();
        
        window.addEventListener('resize', handleResize);
        window.addEventListener('mousemove', handleMouseMove);
        window.addEventListener('touchmove', handleTouchMove, { passive: true });
        window.addEventListener('touchstart', handleTouchMove, { passive: true });
        window.addEventListener('mouseout', handleInteractionEnd);
        window.addEventListener('touchend', handleInteractionEnd);
    }
});

onUnmounted(() => {
    window.removeEventListener('resize', handleResize);
    window.removeEventListener('mousemove', handleMouseMove);
    window.removeEventListener('touchmove', handleTouchMove);
    window.removeEventListener('touchstart', handleTouchMove);
    window.removeEventListener('mouseout', handleInteractionEnd);
    window.removeEventListener('touchend', handleInteractionEnd);
    cancelAnimationFrame(animationFrameId);
});
</script>

<template>
    <canvas ref="canvasRef" class="particles-canvas"></canvas>
</template>

<style scoped>
.particles-canvas {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: -1; 
    pointer-events: none; 
    /* Gradient movido para cá para ser o fundo global */
    background: linear-gradient(180deg, #FAF9F6 0%, #FFF5F7 100%);
}
</style>
