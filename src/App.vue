<script setup>
import Header from './components/Header.vue'
import HeroSection from './components/HeroSection.vue'
import Projects from './components/Projects.vue'
import Skills from './components/Skills.vue'
import Contact from './components/Contact.vue'
import { ref, watchEffect } from 'vue'

/*----------
 Back to Top
 ----------*/
const scrollY = ref(window.scrollY)
const isVisible = ref(false)
const isZoomedOut = ref(false)
const scrollThreshold = 300 // Show after scrolling 300px
const zoomOutThreshold = 1200 // Zoom out after scrolling 1200px

const updateScroll = () => {
  scrollY.value = window.scrollY
}

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: "smooth"})
}

watchEffect(() => {
  isVisible.value = scrollY.value > scrollThreshold
  isZoomedOut.value = scrollY.value > zoomOutThreshold
})

window.addEventListener("scroll", updateScroll)
</script>

<template>
  <Header />
  <HeroSection id="hero"/>
  <!-- Parallax 1 Start -->
    <div class="parallax-container">
    <div
      class="parallax-image"
      style="
        background-image: url('./images/city.jpg');">
      <h2 class="parallax-h">skills</h2>
    </div>
  </div>
  <!-- End Parallax -->
  <!-- Skills -->
  <Skills id="skills" />
  <!-- Parallax 2 Start -->
  <div class="parallax-container">
    <div
      class="parallax-image"
      style="
        background-image: url('./images/mountain.jpg');">
      <h2 class="parallax-h">Portfolio</h2>
    </div>
  </div>
  <!-- End Parallax -->
  <!-- Projects -->
  <Projects id="projects"/>
  <!-- Parallax 3 Start -->
  <div class="parallax-container">
    <div
      class="parallax-image"
      style="
        background-image: url('./images/dark.jpg');">
      <h2 class="parallax-h">Contact</h2>
    </div>
  </div>
  <!-- End Parallax -->
  <!-- Contact Form -->
  <Contact id="contact"/>
  <button class="back-to-top" :class="{ '-is-visible': isVisible, '-zoom-out': isZoomedOut }" @click="scrollToTop"></button>
</template>

<style scoped>
/*----------------------------------
  Parallax 
------------------------------------*/
.parallax-container {
  position: relative;
  height: 300px;
  overflow: hidden;
}

.parallax-image {
  position: absolute;
  width: 100%;
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  text-shadow: 2px 2px 4px #111;
  background-position: center;
  background-size: cover;
  box-shadow: inset 0 0 1em #111;
  z-index: 0;
  animation: linear move-background;
  animation-duration: 0.1s;
  animation-timeline: scroll(root block);
}

.parallax-h {
  font-size: 3rem;
}
/* Vertical scroll for wide screen, horizontal scroll for narrow screen*/
@keyframes move-background {
  from {
    background-position: 0% 0%;
  }
  to {
    background-position: 100% 100%;
  }
}
</style>
