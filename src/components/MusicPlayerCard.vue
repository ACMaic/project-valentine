<script setup>
import { ref, computed } from 'vue';
import { Play, Pause, SkipForward, SkipBack } from 'lucide-vue-next';

// Imports de segurança - Certifique-se que esses arquivos existem em src/assets/
import img1 from '../assets/CAPA1.png';
import img2 from '../assets/CAPA2.png';
import img3 from '../assets/CAPA3.png';
import img4 from '../assets/CAPA4.png'; 
import img5 from '../assets/CAPA5.png';

const playlist = ref([
  {
    title: "Quero Ser Feliz Também",
    artist: "Natiruts",
    cover: img1, 
    src: "/quero-ser-feliz-tambem.mp3" 
  },
  {
    title: "Me Apaixonei",
    artist: "Heitor Costa",
    cover: img2,
    src: "/Me Apaixonei.mp3"
  },
  {
    title: "Se Mordendo de Raiva",
    artist: "Xand Avião",
    cover: img3,
    src: "/se-mordendo-de-raiva.mp3"
  },
  {
    title: "Amanhecer",
    artist: "BK', Nansy Silvvz",
    cover: img4,
    src: "/amanhecer.mp3"
  },
  {
    title: "Mais Feliz",
    artist: "Zeca Pagodinho",
    cover: img5,
    src: "/mais-feliz.mp3" 
  }
]);

const currentIndex = ref(0);
const isPlaying = ref(false);
const audioRef = ref(null);
const currentTime = ref(0);
const duration = ref(0);

const currentTrack = computed(() => playlist.value[currentIndex.value]);

// Formatação de tempo (00:00)
const formatTime = (seconds) => {
  if (!seconds) return "0:00";
  const mins = Math.floor(seconds / 60);
  const secs = Math.floor(seconds % 60);
  return `${mins}:${secs.toString().padStart(2, '0')}`;
};

const onTimeUpdate = () => {
    if(audioRef.value) {
        currentTime.value = audioRef.value.currentTime;
    }
};

const onLoadedMetadata = () => {
    if(audioRef.value) {
        duration.value = audioRef.value.duration;
    }
};

const onPlay = () => {
    isPlaying.value = true;
};

const onPause = () => {
    isPlaying.value = false;
};

const seek = (event) => {
  const val = event.target.value;
  if(audioRef.value) {
      audioRef.value.currentTime = val;
  }
};

const togglePlay = async () => {
  if (!audioRef.value) return;
  console.log("Audio Source:", audioRef.value.src);
  
  try {
      if (audioRef.value.paused) {
        await audioRef.value.play();
      } else {
        audioRef.value.pause();
      }
  } catch (e) {
      console.error("Erro ao reproduzir áudio:", e);
  }
  // Removemos a alteração manual de isPlaying.value aqui.
  // Deixamos os eventos @play e @pause cuidarem disso.
};

const nextTrack = () => {
  currentIndex.value = (currentIndex.value + 1) % playlist.value.length;
  resetAndPlay();
};

const prevTrack = () => {
  currentIndex.value = currentIndex.value > 0 ? currentIndex.value - 1 : playlist.value.length - 1;
  resetAndPlay();
};

const resetAndPlay = () => {
  // Pequeno delay para garantir que o src mudou
  setTimeout(() => {
    if(audioRef.value) {
      audioRef.value.src = currentTrack.value.src + '?t=' + Date.now();
      audioRef.value.load(); // Importante: Carrega o novo recurso
      audioRef.value.play().catch(e => console.error("Erro ao tocar nova faixa:", e));
    }
  }, 50);
};
</script>

<template>
  <section class="music-section">
    <div class="music-card-modern">
      <audio 
        ref="audioRef" 
        :src="currentTrack.src" 
        @timeupdate="onTimeUpdate" 
        @loadedmetadata="onLoadedMetadata"
        @ended="nextTrack"
        @play="onPlay"
        @pause="onPause"
      ></audio>

      <div class="cover-container">
        <img :src="currentTrack.cover" class="album-art" :alt="currentTrack.title" />
      </div>

      <div class="track-info">
        <h3 class="track-title">{{ currentTrack.title }}</h3>
        <p class="track-artist">{{ currentTrack.artist }}</p>
      </div>

      <div class="progress-area">
        <input 
          type="range" 
          class="progress-slider" 
          min="0" 
          :max="duration" 
          :value="currentTime" 
          @input="seek"
        />
        <div class="time-info">
          <span>{{ formatTime(currentTime) }}</span>
          <span>{{ formatTime(duration) }}</span>
        </div>
      </div>

      <div class="controls">
        <button class="btn-side" @click="prevTrack"><SkipBack :size="24" /></button>
        <button class="btn-main" @click="togglePlay">
          <Pause v-if="isPlaying" :size="32" />
          <Play v-else :size="32" style="margin-left: 4px;" />
        </button>
        <button class="btn-side" @click="nextTrack"><SkipForward :size="24" /></button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.music-section {
  padding: 40px 20px;
  display: flex;
  justify-content: center;
}

.music-card-modern {
  background: #1E1E1E; /* Grafite profundo */
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 24px;
  padding: 24px;
  width: 100%;
  max-width: 340px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4); /* Sombra densa */
  text-align: center;
  position: relative;
  overflow: hidden;
}

/* Opcional: Efeito sutil de gradiente de fundo */
.music-card-modern::before {
  content: "";
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(212, 165, 154, 0.05) 0%, transparent 70%);
  pointer-events: none;
  z-index: 0;
}

.cover-container {
  width: 220px;
  height: 220px;
  margin: 0 auto 20px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 20px rgba(0,0,0,0.3);
  position: relative;
  z-index: 1;
}

.album-art {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.track-info {
  position: relative;
  z-index: 1;
}

.track-title {
  font-weight: 700;
  font-size: 1.25rem;
  color: #F5F5F5; /* Off-white */
  margin-bottom: 4px;
}

.track-artist {
  font-size: 0.95rem;
  color: #B0B0B0; /* Cinza claro */
  margin-bottom: 24px;
}

.progress-area {
  margin-bottom: 24px;
  position: relative;
  z-index: 1;
}

.progress-slider {
  width: 100%;
  -webkit-appearance: none;
  appearance: none;
  background: rgba(255, 255, 255, 0.1);
  height: 4px;
  border-radius: 2px;
  outline: none;
  cursor: pointer;
}

.progress-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: #D4A59A; /* Rosa Seco */
  cursor: pointer;
  transition: transform 0.2s;
  box-shadow: 0 0 10px rgba(212, 165, 154, 0.5); /* Glow no thumb */
}

.progress-slider::-webkit-slider-thumb:hover {
  transform: scale(1.2);
}

.time-info {
  display: flex;
  justify-content: space-between;
  font-size: 0.75rem;
  color: #888;
  margin-top: 8px;
  font-weight: 500;
}

.controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 24px;
  position: relative;
  z-index: 1;
}

.btn-side {
  background: none;
  border: none;
  cursor: pointer;
  color: #CCCCCC;
  transition: all 0.2s;
}

.btn-side:hover {
  color: #FFFFFF;
  transform: scale(1.1);
}

.btn-main {
  background: #D4A59A;
  border: none;
  width: 64px;
  height: 64px;
  border-radius: 50%;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 0 20px rgba(212, 165, 154, 0.4); /* Glow externo */
  transition: transform 0.2s, box-shadow 0.2s;
}

.btn-main:hover {
  transform: scale(1.05);
  box-shadow: 0 0 25px rgba(212, 165, 154, 0.6);
}
</style>