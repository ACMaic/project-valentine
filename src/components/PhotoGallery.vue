<script setup>
</script>

<template>
  <section class="gallery-section container">
    <h2 class="section-title text-center">Us</h2>
    <div class="gallery-grid">
      <!-- In a real app, these would be real images. Using styled placeholders for now. -->
      <div class="gallery-item item-1"></div>
      <div class="gallery-item item-2"></div>
      <div class="gallery-item item-3"></div>
      <div class="gallery-item item-4"></div>
    </div>
  </section>
</template>

<style scoped>
.gallery-section {
  padding: var(--spacing-lg) 0;
}

.section-title {
  font-size: 2.5rem;
  margin-bottom: var(--spacing-lg);
  color: var(--color-primary);
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: var(--spacing-sm);
  max-width: 800px;
  margin: 0 auto;
}

.gallery-item {
  background-color: #eee;
  border-radius: var(--radius-md);
  width: 100%;
  aspect-ratio: 1/1;
  background-size: cover;
  background-position: center;
  position: relative;
  overflow: hidden;
  box-shadow: var(--shadow-md);
}

/* Simulating images with gradients or colors */
.item-1 { background: radial-gradient(circle, #fde4cf 0%, #faaca8 100%); }
.item-2 { background: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%); }
.item-3 { background: linear-gradient(to top, #a18cd1 0%, #fbc2eb 100%); }
.item-4 { background: linear-gradient(to right, #ffecd2 0%, #fcb69f 100%); }

.gallery-item:hover {
  filter: brightness(0.95);
}

@media (min-width: 600px) {
  .gallery-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>
