<script setup>
import { ref, reactive, computed, useTemplateRef, watch, nextTick, onMounted } from 'vue'

const projects = ref([
  { id: 1, name: "Portfolio Website", languages: ["JavaScript", "SCSS", "CSS", "HTML"], frameworks: ["Vue"], features: [], img: "https://images.unsplash.com/photo-1729541777356-e28f6cd67fb6?q=80&w=1970&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A static webpage to demo a few skills and act as a portfolio", link: "https://github.com/QuestMerchant/portfolio-website"},
  { id: 2, name: "Card Trading Website", languages: ["JavaScript", "Python", "SQL", "CSS", "HTML"], frameworks: ["Flask"], features: ["CRUD", "Authorisation"], img: "https://images.unsplash.com/photo-1621568670868-24a7dfc590e9?q=80&w=2071&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A simple website where users open chests, and can trade virtual cards using a virtual currency", link: "https://questmerchant.pythonanywhere.com/"},
  //{ id: 3, name: "Nelnorth Wiki", languages: ["JavaScript", "SCSS", "CSS", "HTML"], frameworks: ["Vue"], features: [], img: "https://images.unsplash.com/photo-1516780236580-ef416334d5b4?q=80&w=1996&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A wiki based off of a TTRPG <br> Work in Progress"},
  //{ id: 4, name: "Nelnorth App", languages: ["QML", "Python", "NoSQL", "JSON"], frameworks: ["Qt", "PySide"], features: ["Singleton", "Factory Method", "CRUD", "Google Firebase", "Authorisation", "Automation"], img: "https://images.unsplash.com/photo-1524135329990-07660cd5bf10?q=80&w=1965&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A digital toolset for a TTRPG to assist players and automatically save their progress. Similar to dndbeyond.com<br>Work in Progress"},
  { id: 5, name: "Zombie Outbreak Simulation", languages: ["Python"], frameworks: ["Numpy"], features: ["Graphing", "Data Visualisation"], img: "https://plus.unsplash.com/premium_photo-1676673189362-9aa3569e82d9?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTM3fHxncmFwaHxlbnwwfHwwfHx8MA%3D%3D", summary: "A visualisation of survivabilty based on population density during an outbreak", link: "https://github.com/QuestMerchant/Zombie-data-simulation"},
  //{ id: 6, name: "Silent Night Website", languages: ["JavaScript", "SCSS", "CSS", "HTML", "Python"], frameworks: ["Vue", "Flask", "Pinia", "Redis"], features: ["Real-time", "Redis storage", "Web sockets", "Cookies"], img: "https://images.unsplash.com/photo-1567263361507-83f755d9fa97?q=80&w=1976&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A real-time multiplayer game with live chat functionality based on werewolf."}
])
const dropdown = reactive({ height: 0 })
const filters = reactive({
  languages: {},
  frameworks: {},
  features: {}
})
const filterMenus = reactive({
  languages: false,
  frameworks: false,
  features: false
})
const filterMenuRef = useTemplateRef("filterMenu")


// Extract unique data from projects
function initializeFilters() {
  projects.value.forEach(project => {
    project.languages.forEach(lang => {
      if (!(lang in filters.languages)) {
        filters.languages[lang] = false
      }
    }),
    project.frameworks.forEach(frame => {
      if (!(frame in filters.frameworks)) {
        filters.frameworks[frame] = false
      }
    }),
    project.features.forEach(feat => {
      if (!(feat in filters.features)) {
        filters.features[feat] = false
      }
    })
  })
}
onMounted(() => {
  initializeFilters()
})

/*-----------
  Filter
-----------*/
const activeMenu = computed(() => {
  return Object.keys(filterMenus).reduce((a, menu, i) => (
    // Check if current menu is active/true, set to index otherwise keep original accumulator
    filterMenus[menu] ? i : a
    //initialValue must be -1 in case of no active menu
  ), -1)
})

const activeFilters = computed(() => {
  return {
    languages: Object.keys(filters.languages).filter(lang => filters.languages[lang]),
    frameworks: Object.keys(filters.frameworks).filter(frame => filters.frameworks[frame]),
    features: Object.keys(filters.features).filter(feat => filters.features[feat])
  }
})

function clearAllFilters() {
  Object.keys(activeFilters.value).forEach(menu => {
    Object.keys(filters[menu]).forEach(key => {
      filters[menu][key] = false
    })
  })
}

function setMenu(menu, active) {
  Object.keys(filterMenus).forEach(tab => {
    filterMenus[tab] = !active && tab === menu
  })
}

function setFilter(filter, option) {
  filters[filter][option] = !filters[filter][option]
}

async function updateDropdown(index, from) {
  if (index === from) return
  await nextTick()
  if(!filterMenuRef.value || !filterMenuRef.value[index]) {
    dropdown.height = 0
  }
  else {
    dropdown.height = filterMenuRef.value[index].clientHeight + 16 + "px"
  }
}

watch(activeMenu, updateDropdown)

const isFilterActive = computed (() =>
  Object.values(activeFilters.value).some(arr => arr.length > 0)
)

const filteredProjects = computed(() => {
  return isFilterActive.value
    ? projects.value.filter(project =>
      Object.keys(activeFilters.value).every(key =>
        activeFilters.value[key].length === 0
          ? true
          : Array.isArray(project[key])
            ? activeFilters.value[key].some(filter => project[key].includes(filter))
            : activeFilters.value[key].includes(project[key])
      )
    )
    : projects.value
})
</script>

<template>
  <div>
    <section class="p-4 bg-linear-to-b from-gray-50 to-gray-100 shadow-sm dark:from-gray-900 dark:to-gray-800 dark:text-gray-200">
      <!-- Portfolio Filter -->
      <!-- Filter Tabs Start -->
      <nav class="border-b border-gray-200 dark:border-gray-700 pb-3">
        <div class="flex flex-wrap items-center gap-2">
          <i class="icon ti-filter"></i>
          <button v-for="(active, menu) in filterMenus"
            class="px-3 py-1 text-sm rounded capitalize transition-colors"
            :class="active
              ? 'bg-blue-600 text-white'
              : 'bg-gray-200 text-gray-700 dark:bg-gray-700 dark:text-gray-300 hover:bg-gray-300 dark:hover:bg-gray-600'"
            @click="setMenu(menu, active)">
            {{ menu }}&nbsp;<span v-if="activeFilters[menu].length > 0" class="bg-green-500 text-white text-xs rounded-full px-1.5 ml-1">{{ activeFilters[menu].length }}</span>
          </button>
          <button class="px-3 py-1 text-sm rounded bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-200 hover:bg-yellow-200 dark:hover:bg-yellow-800 transition-colors" type="button" @click="clearAllFilters">
            Clear all
          </button>
        </div>
        <!-- <label style="cursor: pointer" data-bs-toggle="modal" data-bs-target="#portfolioModal" class="text-blue-600 dark:text-blue-400 hover:underline mt-2 inline-block">View More!</label> -->
      </nav>
      <!-- Filter Tabs End-->
      <!-- Filter Options Start -->
      <transition-group name="dropdown" tag="div" class="dropdown" :style="dropdown">
        <menu v-for="(options, filter) in filters" v-show="filterMenus[filter]" ref="filterMenu" :key="filter" class="filters">
          <button type="button" v-for="(active, option) in options" @click="setFilter(filter, option)" class="filters__btn px-3 py-1 text-sm rounded transition-colors" :class="active ? 'bg-blue-600 text-white' : 'bg-gray-200 text-gray-700 dark:bg-gray-700 dark:text-gray-300 hover:bg-gray-300 dark:hover:bg-gray-600'">
            {{ option }}
          </button>
        </menu>
      </transition-group>
      <!-- Filter Options End -->
      <!-- End Portfolio Filter -->
      <!-- Display Results Start -->
      <div class="mb-5">
        <transition-group name="project" tag="div" class="flex flex-wrap gap-4 justify-center">
          <div class="min-w-60 max-w-sm flex-1" v-for="project in filteredProjects" :key="project.id">
            <div class="project">
              <div class="portfolio-cover" :style="{ backgroundImage: `url(${project.img})`}">
                <div class="text-white p-3">
                  <p>Languages: {{project.languages}}</p>
                  <h3>{{project.name}}</h3>
                </div>
              </div>
              <div class="portfolio-hover">
                <div class="mb-1 text-gray-50">
                  <h4>Summary</h4>
                  <p>{{project.summary}}</p>
                </div>
                <a :href="project.link" class="inline-flex items-center justify-center w-8 h-8 rounded-full bg-white text-blue-600 hover:bg-blue-100 transition-colors">
                  <i class="ti-link"></i>
                </a>
              </div>
            </div>
          </div>
        </transition-group>
      </div>
      <!-- Display Results End -->
      <!-- Modal 
      <div class="modal fade" id="portfolioModal" tabindex="-1" aria-labelledby="portfolioModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title" id="portfolioModalLabel">Github Projects</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="close"></button>
            </div>
            <div class="modal-body">
              <p>To see my public repos, click <a href="https://github.com/QuestMerchant?tab=repositories">here</a></p>
            </div>
          </div>
        </div>
      </div> -->
    </section>
  </div>
</template>

<style scoped lang="scss">
/*------------------
  Dropdown
 ------------------*/
.dropdown {
  position: relative;
  overflow: hidden;
  height: 0;
  transition: height 200ms ease;

  &::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 0.2rem;
  }

  &-enter,
  &-leave-to { opacity: 0 }

  &-leave,
  &-enter-to { opacity: 1}

  &-enter-active,
  &-leave-active {
    position: absolute;
    width: 100%;
    transition: opacity 200ms ease-in-out
  }

  &-enter-active { transition-delay: 100ms }
}

/*------------------
  Cards 
 ------------------*/

.project {
  position: relative;
  height: 240px;
  border-radius: 6px;
  box-shadow: 0 0 0 1px #111;
  backface-visibility: hidden;
  transform-origin: 10% 50%;
  z-index: 1;
  overflow: hidden;

  /* items moving that stay visible */
  &-move {
    transition: all 500ms ease-in-out 100ms;
  }

  /* appearing */
  &-enter-active {
    transition: all 500ms ease-out;
  }

  /* disappearing */
  &-leave-active {
    transition: all 250ms ease-in;
    position: absolute;
    z-index: 0;
  }

  &-enter,
  &-leave-to {
    opacity: 0;
  }
}

.portfolio-cover {
  position: relative;
  display: block;
  padding-top: 0.75rem;
  padding-left: 1rem;
  padding-right: 1rem;
  height: 100%;
  background-size: cover;
  background-position: center;
  text-shadow: 0px 0px 12px rgb(0, 0, 0);

  p, h3 {
    position: relative;
    z-index: 1;
  }

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.3);
    border-radius: 6px;
  }

  &::after {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    width: 100%;
    height: 100%;
    border-radius: 6px;
    background: rgba(13, 110, 253, 0.85);
    content: " ";
    -webkit-transform: translate3d(0, 100%, 0);
    transform: translate3d(0, 100%, 0);
    transition-duration: 400ms;
    transition-property: all;
    transition-timing-function: cubic-bezier(0.7, 1, 0.7, 1);
  }
}

.project:hover .portfolio-cover:after {
  -webkit-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0);
}

.portfolio-hover {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  text-align: center;
  padding: 1.875rem;
  opacity: 0;
  transition-duration: 400ms;
  transition-property: all;
  transition-timing-function: cubic-bezier(0.7, 1, 0.7, 1);
}

.project:hover .portfolio-hover {
  opacity: 1;
}

.text-white {
  opacity: 1;
  transition: opacity 400ms;
}
.project:hover .text-white {
  opacity: 0;
}
</style>
