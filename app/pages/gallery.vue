<script setup lang="ts">
useSeoMeta({
  title: 'Our Projects Gallery - PAC Construction Solutions LTD',
  description: 'View our portfolio of completed tape & jointing and plasterboard finishing projects across London.',
})

// Массив ссылок на изображения с Google Диска
// Формат ссылки: https://drive.google.com/uc?export=view&id=FILE_ID
// Где FILE_ID - это ID файла из ссылки на Google Диск
const images = ref([
  // Правильный формат: https://drive.google.com/uc?export=view&id=FILE_ID
  // Берёшь ID из ссылки (между /d/ и /view) и вставляешь сюда:
  'https://lh3.googleusercontent.com/d/1BzaK_InaTyJ85UnmL56Fow1UPL3ECasi',
  'https://drive.google.com/thumbnail?id=1WAHpMQq0WpuCzgpvtnM6SNW6smukulRK&sz=w1000',
  'https://drive.google.com/thumbnail?id=1BKo51T4BlVUoVywgoXBEZ9gRkXZRYhWN&sz=w1000',
  'https://drive.google.com/thumbnail?id=1up5cbw6yNjiCWsk5UVnR6fX7t8UQscHt&sz=w1000',
  'https://drive.google.com/thumbnail?id=1J28Sj3hlIZTcG92RqfQ-4nF2UbS-XzZv&sz=w1000',
  'https://drive.google.com/thumbnail?id=1mgwj-sPgI-TlLbnv2KoV-bu4xZZRMXf0&sz=w1000',
  'https://drive.google.com/thumbnail?id=1OS04Tn22OOsQyUs2DFdmo-yIHmIyiuQy&sz=w1000',
  'https://drive.google.com/thumbnail?id=1Nx-zL9Y8XzzTqhprvkJCPh_pVVMQ6AAA&sz=w1000',
  'https://drive.google.com/thumbnail?id=16OwL0ak9xXlrnuEteg-pCTfcyA7DiLnk&sz=w1000',
  'https://drive.google.com/thumbnail?id=15HH6TJuMOmNPUY5IHbArBBxlE-sYL4nJ&sz=w1000',
  'https://drive.google.com/thumbnail?id=1HIE5pyNMgsVwkGV_o2Q8DlI8k5dE8rQV&sz=w1000',
  'https://drive.google.com/thumbnail?id=1Zdr07WI7czzXCZR9zD_wUA2yoI3yZm7H&sz=w1000',
  'https://drive.google.com/thumbnail?id=1b2OzhTSIzgoK1gl7TdrZzv6QL7jrTzYD&sz=w1000',
  'https://drive.google.com/thumbnail?id=1zGv7UTFdv5Ju0YAuO69dqWHv9RjkPUER&sz=w1000',
  'https://drive.google.com/thumbnail?id=1XM6LejzxouIfA9qchtepPy3mCs4jT6G2&sz=w1000',
  'https://drive.google.com/thumbnail?id=1RG1uXuK4lvaOSHgiRxMLXXEHuODOZqe4&sz=w1000',
  'https://drive.google.com/thumbnail?id=1lelVw5St-EhhAn82l4LIVG2hUN7tC5QJ&sz=w1000',

  
])

const currentIndex = ref(0)
const isTransitioning = ref(false)
const touchStartX = ref(0)
const touchEndX = ref(0)

const totalSlides = computed(() => images.value.length)

const nextSlide = () => {
  if (isTransitioning.value) return
  isTransitioning.value = true
  currentIndex.value = (currentIndex.value + 1) % totalSlides.value
  setTimeout(() => isTransitioning.value = false, 500)
}

const prevSlide = () => {
  if (isTransitioning.value) return
  isTransitioning.value = true
  currentIndex.value = (currentIndex.value - 1 + totalSlides.value) % totalSlides.value
  setTimeout(() => isTransitioning.value = false, 500)
}

const goToSlide = (index: number) => {
  if (isTransitioning.value || index === currentIndex.value) return
  isTransitioning.value = true
  currentIndex.value = index
  setTimeout(() => isTransitioning.value = false, 500)
}

// Touch/Swipe support
const handleTouchStart = (e: TouchEvent) => {
  if (e.touches[0]) touchStartX.value = e.touches[0].clientX
}

const handleTouchMove = (e: TouchEvent) => {
  if (e.touches[0]) touchEndX.value = e.touches[0].clientX
}

const handleTouchEnd = () => {
  const diff = touchStartX.value - touchEndX.value
  const threshold = 50
  
  if (Math.abs(diff) > threshold) {
    if (diff > 0) {
      nextSlide()
    } else {
      prevSlide()
    }
  }
}

// Keyboard navigation
const handleKeydown = (e: KeyboardEvent) => {
  if (e.key === 'ArrowRight') nextSlide()
  if (e.key === 'ArrowLeft') prevSlide()
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})

// Autoplay (optional - можно раскомментировать)
// const autoplayInterval = ref<ReturnType<typeof setInterval> | null>(null)
// onMounted(() => {
//   autoplayInterval.value = setInterval(nextSlide, 5000)
// })
// onUnmounted(() => {
//   if (autoplayInterval.value) clearInterval(autoplayInterval.value)
// })
</script>

<template>
  <div class="gallery-page">
    <OPageHero 
      title="Our Projects"
      subtitle="Browse our portfolio of completed tape & jointing and plasterboard finishing work"
      background-image="https://images.unsplash.com/photo-1621905251189-08b45d6a269e?w=1600&h=600&fit=crop"
    />
    
    <section class="slider-section section">
      <div class="container">
        <div class="section__header">
          <h2 class="section__title">Project Gallery</h2>
          <p class="section__subtitle">Swipe through our completed projects</p>
        </div>
        <ClientOnly>
          <div 
            class="slider"
            @touchstart="handleTouchStart"
            @touchmove="handleTouchMove"
            @touchend="handleTouchEnd"
          >
            <!-- Main Slider -->
            <div class="slider__viewport">
              <div 
                class="slider__track"
                :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
              >
                <div 
                  v-for="(image, index) in images" 
                  :key="index"
                  class="slider__slide"
                >
                  <img 
                    :src="image" 
                    :alt="`Project ${index + 1}`"
                    class="slider__image"
                    loading="lazy"
                  />
                </div>
              </div>
            </div>
            
            <!-- Navigation Arrows -->
            <button 
              class="slider__nav slider__nav--prev" 
              @click="prevSlide"
              :disabled="isTransitioning"
              aria-label="Previous slide"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="15 18 9 12 15 6"></polyline>
              </svg>
            </button>
            
            <button 
              class="slider__nav slider__nav--next" 
              @click="nextSlide"
              :disabled="isTransitioning"
              aria-label="Next slide"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="9 18 15 12 9 6"></polyline>
              </svg>
            </button>
            
            <!-- Counter -->
            <div class="slider__counter">
              <span class="slider__counter-current">{{ String(currentIndex + 1).padStart(2, '0') }}</span>
              <span class="slider__counter-separator">/</span>
              <span class="slider__counter-total">{{ String(totalSlides).padStart(2, '0') }}</span>
            </div>
          </div>
        </ClientOnly>
        
        <!-- Dots Navigation -->
        <div class="slider__dots">
          <button 
            v-for="(_, index) in images"
            :key="index"
            class="slider__dot"
            :class="{ 'slider__dot--active': currentIndex === index }"
            @click="goToSlide(index)"
            :aria-label="`Go to slide ${index + 1}`"
          />
        </div>
        
        <!-- Thumbnails -->
        <div class="slider__thumbnails">
          <button 
            v-for="(image, index) in images"
            :key="index"
            class="slider__thumbnail"
            :class="{ 'slider__thumbnail--active': currentIndex === index }"
            @click="goToSlide(index)"
          >
            <img :src="image" :alt="`Thumbnail ${index + 1}`" loading="lazy" />
          </button>
        </div>
      </div>
    </section>
    
    <OCta />
  </div>
</template>

<style scoped>
.slider-section {
  background: linear-gradient(180deg, var(--color-light) 0%, var(--color-white) 100%);
}

.slider {
  position: relative;
  max-width: 1000px;
  margin: 0 auto;
  border-radius: var(--border-radius-xl);
  overflow: hidden;
  box-shadow: var(--shadow-xl);
  background: var(--color-dark);
}

.slider__viewport {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 10;
  overflow: hidden;
}

.slider__track {
  display: flex;
  height: 100%;
  transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.slider__slide {
  flex: 0 0 100%;
  width: 100%;
  height: 100%;
}

.slider__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

/* Navigation Arrows */
.slider__nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 56px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  color: white;
  cursor: pointer;
  transition: all var(--transition-normal);
  z-index: 10;
}

.slider__nav:hover:not(:disabled) {
  background: var(--color-primary);
  color: var(--color-dark);
  transform: translateY(-50%) scale(1.1);
  box-shadow: 0 0 30px rgba(245, 166, 35, 0.4);
}

.slider__nav:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.slider__nav--prev {
  left: 20px;
}

.slider__nav--next {
  right: 20px;
}

/* Counter */
.slider__counter {
  position: absolute;
  bottom: 20px;
  left: 20px;
  display: flex;
  align-items: baseline;
  gap: 4px;
  padding: 10px 18px;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(10px);
  border-radius: 30px;
  font-family: var(--font-heading);
  color: white;
  z-index: 10;
}

.slider__counter-current {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--color-primary);
}

.slider__counter-separator {
  font-size: 1rem;
  opacity: 0.5;
  margin: 0 2px;
}

.slider__counter-total {
  font-size: 1rem;
  font-weight: 500;
  opacity: 0.7;
}

/* Dots Navigation */
.slider__dots {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 30px;
}

.slider__dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--color-secondary);
  opacity: 0.3;
  border: none;
  cursor: pointer;
  transition: all var(--transition-normal);
}

.slider__dot:hover {
  opacity: 0.6;
  transform: scale(1.2);
}

.slider__dot--active {
  opacity: 1;
  background: var(--color-primary);
  transform: scale(1.3);
  box-shadow: 0 0 15px rgba(245, 166, 35, 0.5);
}

/* Thumbnails */
.slider__thumbnails {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 25px;
  flex-wrap: wrap;
}

.slider__thumbnail {
  width: 80px;
  height: 60px;
  border-radius: var(--border-radius);
  overflow: hidden;
  cursor: pointer;
  border: 3px solid transparent;
  opacity: 0.6;
  transition: all var(--transition-normal);
}

.slider__thumbnail:hover {
  opacity: 0.9;
  transform: translateY(-3px);
}

.slider__thumbnail--active {
  opacity: 1;
  border-color: var(--color-primary);
  box-shadow: 0 4px 15px rgba(245, 166, 35, 0.4);
}

.slider__thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Responsive */
@media (max-width: 768px) {
  .slider__nav {
    width: 44px;
    height: 44px;
  }
  
  .slider__nav--prev {
    left: 10px;
  }
  
  .slider__nav--next {
    right: 10px;
  }
  
  .slider__counter {
    bottom: 15px;
    left: 15px;
    padding: 8px 14px;
  }
  
  .slider__counter-current {
    font-size: 1.2rem;
  }
  
  .slider__thumbnails {
    gap: 10px;
  }
  
  .slider__thumbnail {
    width: 60px;
    height: 45px;
  }
}

@media (max-width: 480px) {
  .slider__viewport {
    aspect-ratio: 4 / 3;
  }
  
  .slider__nav {
    width: 38px;
    height: 38px;
  }
  
  .slider__nav svg {
    width: 18px;
    height: 18px;
  }
  
  .slider__dots {
    gap: 8px;
  }
  
  .slider__dot {
    width: 10px;
    height: 10px;
  }
  
  .slider__thumbnail {
    width: 50px;
    height: 38px;
  }
}
</style>
