<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { register } from 'swiper/element/bundle'

const text = 'Aspiring Full-Stack Developer'
const description = ref('')
const isTyping = ref(true)
let index = 0

const typeEffect = () => {
  if (index < text.length) {
    description.value += text.charAt(index)
    index++
    setTimeout(typeEffect, 200)
  }
  else {
    // Allow blinking for addintional time
    setTimeout(() => {
      isTyping.value = false
    }, 2000)
  }
}
onMounted(() => {
  setTimeout(typeEffect, 700) 
})

/*---------------
  Swiper
----------------*/
register()


/*---------------
  Smooth Scroll
----------------*/
const scrollToSection = (sectionId) => {
  const targetElement = document.getElementById(sectionId)
  if (targetElement) {
    targetElement.scrollIntoView({
      behavior: "smooth",
      block: "start",
    })
  }
}

/*---------------
  Video Modal
----------------*/
const videoModal = ref(null)
const videoIframe = ref(null)
const videoUrl = ref("https://www.youtube.com/embed/dQw4w9WgXcQ")

const loadVideo = () => {
  if (videoIframe.value) {
    videoIframe.value.src = `${videoUrl.value}?autoplay=1`
  }
}

const stopVideo = () => {
  if (videoIframe.value) {
    videoIframe.value.src = ""
  }
}

onMounted(() => {
  if (videoModal.value) {
    videoModal.value.addEventListener("show.bs.modal", loadVideo)
    videoModal.value.addEventListener("hidden.bs.modal", stopVideo)
  }
})

// Remove event listeners when component is unmounted
onUnmounted(() => {
  if (videoModal.value) {
    videoModal.value.removeEventListener("show.bs.modal", loadVideo)
    videoModal.value.removeEventListener("hidden.bs.modal", stopVideo)
  }
})
</script>

<template>
  <section class="hero c-swiper">
    <!-- Swiper Wrapper -->
    <swiper-container class="mySwiper" :parallax="true" speed="400"  navigation="true" :pagination="{dynamicBullets: true}">
      <div slot="container-start" class="parallax-bg" style="background-image: url('./images/keyboard.jpg')" data-swiper-parallax="-20%"></div>
      <swiper-slide>
        <div class="container" data-swiper-parallax="-300">
          <h1 class="hero-title">Welcome to My Portfolio</h1>
          <p class="hero-subtitle">
            {{ description }}<span v-if="isTyping" class="cursor">|</span>
          </p>
          <button class="hero-button" @click="scrollToSection('projects')">View My Work</button>
        </div>
      </swiper-slide>
      <swiper-slide>
        <div class="container" data-swiper-parallax="-300">
          <h1>A smooth experience<br />for both mobile <br />and desktop users</h1>
        </div>
      </swiper-slide>
      <swiper-slide>
        <div class="container" data-swiper-parallax="-300">
          <div>
            <h2>
              Lorem ipsum dolor sit amet,<br />consectetur adipiscing elit.<br />Aliquam et ultricies leo. 
            </h2>
          </div>
          <a
            data-bs-toggle="modal"
            data-bs-target="#videoModal"
            href="#"
            title="Best Video"
          >
            <i class="icon icon--lg icon--white-bg radius--circle ti-control-play"></i>
          </a>
          
        </div>
      </swiper-slide>
    </swiper-container>
    <!-- End Swiper Wrapper-->
    <!-- Pop up Video -->
    <div ref="videoModal" id="videoModal" class="modal" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-lg">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <iframe 
            ref="videoIframe"
            width="100%" 
            height="400" 
            :src="videoUrl" 
            frameborder="0" 
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
            referrerpolicy="strict-origin-when-cross-origin" 
            allowfullscreen></iframe>
        </div>
      </div>
    </div>
  </div>
    <!-- Arrows 
    <a
      href="javascript:void(0);"
      class="c-swiper__arrow--right icon icon--md icon--white-brd radius--circle ti-angle-right"
      @click="nextSlide()"
    ></a>
    <button
      type="button"
      @click="nextSlide()"
      class="c-swiper__arrow--left icon icon--md icon--white-brd radius--circle ti-angle-left"
    ></button>
     End Arrows -->

    <!-- Scroll to Skills Section -->
    <a href="#skills" class="scroll-to-section hero-content" @click.prevent="scrollToSection('skills')">
      <span class="ti-angle-double-down icon--md"></span>
      <p class="hero-subtitle">Skills</p>
    </a>
  </section>
</template>

<style scoped lang="scss">
.hero {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(to right, #4facfe, #00f2fe);
  color: #ffffff;
  text-align: center;
  padding: 0 20px;
}

.hero-content {
  color: white;
  max-width: 600px;
  margin-bottom: 1.875rem;
}

.hero-title {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  animation: fadeIn 1s ease-in-out;
}

.hero-subtitle {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  animation: fadeIn 1.5s ease-in-out;
}

.hero-button {
  background-color: #ff7a59;
  color: #ffffff;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  animation: fadeIn 2s ease-in-out;
  transition: background-color 0.3s ease;
}

.hero-button:hover {
  background-color: #ff5733;
}

/*----------------------------------
  Swiper
------------------------------------*/
swiper-container {
  height: 100%;
  width: 100%;

  &::part(button-next) {
    
  }

  &::part(button-next),
  &::part(button-prev) {
    color: red;
    width: 2rem;
    height: 2rem;
    padding: 0.75rem;
    display: flex;
    position: absolute;
    top: 50%;
    z-index: 1;
    -webkit-transform: translate3d(0, -50%, 0);
    transform: translate3d(0, -50%, 0);
    border: solid;
    border-color: white;
    border-radius: 50%;
    border-width: 0.0625rem;

    &:hover{
      background-color: rgba(12, 12, 12, 0.3)
    }
  }
}

swiper-slide {
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}

.parallax-bg {
  position: absolute;
  left: 0;
  top: 0;
  width: 130%;
  height: 100%;
  -webkit-background-size: cover;
  background-size: cover;
  background-position: center;
}

.c-swiper {
  position: relative;
  width: 100%;
  overflow: hidden;
}
// No arrows for mobile
@media (max-width: 47.9em) {
  swiper-container::part(button-next),
  swiper-container::part(button-prev) {
    opacity: 0;
  }
}

/* Disappearing arrows
@media (max-width: 47.9em) {
  .c-swiper:hover swiper-container::part(button-next),
  .c-swiper:hover swiper-container::part(button-prev) {
    opacity: 1;
  }
}
*/

/*----------------------------------
  Typewriter
------------------------------------*/
.typewriter {
  white-space: pre-wrap;
  overflow: hidden;
  border-right: 2px solid #000000;
}

.cursor {
  display: inline-block;
  width: 1px;
  animation: blink 1s steps(2, start) infinite;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes blink {
  100% {
    opacity: 0;
  }
}
</style>
