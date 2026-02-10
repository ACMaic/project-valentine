<script setup>
import { ref } from 'vue';
import WelcomeScreen from './components/WelcomeScreen.vue';
import Hero from './components/Hero.vue';
import StorytellingCarousel from './components/StorytellingCarousel.vue';
import Timeline from './components/Timeline.vue';
import ReasonsGrid from './components/ReasonsGrid.vue';
import MusicPlayerCard from './components/MusicPlayerCard.vue'; // New Unified Player
import Footer from './components/Footer.vue';
import BackgroundParticles from './components/BackgroundParticles.vue';

const isUnlocked = ref(false);

const handleUnlock = () => {
    isUnlocked.value = true;
};
</script>

<template>
  <div class="app-container">
    <transition name="fade-screen" mode="out-in">
        <WelcomeScreen v-if="!isUnlocked" @unlocked="handleUnlock" />
        
        <div v-else class="main-content">
            <Hero />
            <StorytellingCarousel />
            <Timeline />
            <ReasonsGrid />
            <MusicPlayerCard />
            <Footer />
        </div>
    </transition>
    <BackgroundParticles />
  </div>
</template>

<style>
/* ... imports ... */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400&display=swap');

.app-container {
  /* ... existing styles ... */
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  /* Global padding for mobile (px-4 equivalent) */
  padding-left: 1rem;
  padding-right: 1rem;
  overflow-x: hidden; 
}

/* Screen Transitions */
.fade-screen-enter-active,
.fade-screen-leave-active {
  transition: opacity 1s ease;
}

.fade-screen-enter-from,
.fade-screen-leave-to {
  opacity: 0;
}


/* Remove padding for full-width sections if needed, but keeping it simple for now */
/* Hero might need negative margins to span full width if inside this container */

</style>

