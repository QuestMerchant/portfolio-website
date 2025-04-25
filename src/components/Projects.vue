<script setup>
import { ref, reactive, computed, useTemplateRef, watch, nextTick, onMounted } from 'vue'

const projects = ref([
  { id: 1, name: "Portfolio Website", languages: ["JavaScript", "SCSS", "CSS", "HTML"], frameworks: ["Vue"], features: [], img: "https://images.unsplash.com/photo-1729541777356-e28f6cd67fb6?q=80&w=1970&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A static webpage to demo a few skills and act as a portfolio", link: "https://github.com/QuestMerchant/portfolio-website"},
  { id: 2, name: "Card Trading Website", languages: ["JavaScript", "Python", "SQL", "CSS", "HTML"], frameworks: ["Flask"], features: ["CRUD", "Authorisation"], img: "https://images.unsplash.com/photo-1621568670868-24a7dfc590e9?q=80&w=2071&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A simple website where users open chests, and can trade virtual cards using a virtual currency", link: "https://questmerchant.pythonanywhere.com/"},
  { id: 3, name: "Nelnorth Wiki", languages: ["JavaScript", "SCSS", "CSS", "HTML"], frameworks: ["Vue"], features: [], img: "https://images.unsplash.com/photo-1516780236580-ef416334d5b4?q=80&w=1996&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A wiki based off of a TTRPG <br> Work in Progress"},
  { id: 4, name: "Nelnorth App", languages: ["QML", "Python", "NoSQL", "JSON"], frameworks: ["Qt", "PySide"], features: ["Singleton", "Factory Method", "CRUD", "Google Firebase", "Authorisation", "Automation"], img: "https://images.unsplash.com/photo-1524135329990-07660cd5bf10?q=80&w=1965&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A digital toolset for a TTRPG to assist players and automatically save their progress. Similar to dndbeyond.com<br>Work in Progress"},
  { id: 5, name: "Zombie Outbreak Simulation", languages: ["Python"], frameworks: ["Numpy"], features: ["Graphing", "Data Visualisation"], img: "https://plus.unsplash.com/premium_photo-1676673189362-9aa3569e82d9?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTM3fHxncmFwaHxlbnwwfHwwfHx8MA%3D%3D", summary: "A visualisation of survivabilty based on population density during an outbreak", link: "https://github.com/QuestMerchant/Zombie-data-simulation"},
  { id: 6, name: "Silent Night Website", languages: ["JavaScript", "SCSS", "CSS", "HTML", "Python"], frameworks: ["Vue", "Flask", "Pinia", "Redis"], features: ["Real-time", "Redis storage", "Web sockets", "Cookies"], img: "https://images.unsplash.com/photo-1567263361507-83f755d9fa97?q=80&w=1976&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D", summary: "A real-time multiplayer game with live chat functionality based on werewolf."}
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
    // Process Languages
    project.languages.forEach(lang => {
      if (!(lang in filters.languages)) {
        filters.languages[lang] = false
      }
    }),
    // Process Frameworks
    project.frameworks.forEach(frame => {
      if (!(frame in filters.frameworks)) {
        filters.frameworks[frame] = false
      }
    }),
    // Process Features
    project.features.forEach(feat => {
      if (!(feat in filters.features)) {
        filters.features[feat] = false
      }
    })
  })
}
onMounted(() =>{
  initializeFilters()
})

/*-----------
  Filter
-----------*/
const activeMenu = computed(() => {
  // Iterate through array, return the last active index
  // .reduce((accumulator, currentValue, index) => func, initialValue)
  return Object.keys(filterMenus).reduce((a, menu, i) => (
    // Check if current menu is active/true, set to index otherwise keep original accumulator
    filterMenus[menu] ? i : a
    //initialValue must be -1 in case of no active menu
  ), -1)
})

const activeFilters = computed(() => {
  return {
    // Return an array of keys where value is true
    languages: Object.keys(filters.languages).filter(lang => filters.languages[lang]),
    frameworks: Object.keys(filters.frameworks).filter(frame => filters.frameworks[frame]),
    features: Object.keys(filters.features).filter(feat => filters.features[feat])
  }
})

function clearAllFilters() {
  // Iterate through activeFilters and set to false.
  Object.keys(activeFilters.value).forEach(menu => {
    Object.keys(filters[menu]).forEach(key => {
      filters[menu][key] = false
    })
  })
}

function setMenu(menu, active) {
  // Iterate through each menu, change current menu, set everything else to false
  Object.keys(filterMenus).forEach(tab => {
    // If either !active or tab===menu results in false, it returns false
    filterMenus[tab] = !active && tab === menu
  })
}

function setFilter(filter, option) {
  filters[filter][option] = !filters[filter][option]
}

/*-----------
  Dropdown
-----------*/
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

/*-----------
  Projects
-----------*/
const isFilterActive = computed (() =>
  Object.values(activeFilters.value).some(arr => arr.length > 0)
)

const filteredProjects = computed(() => {
  // Check if any filters are applied
  return isFilterActive.value
    ? projects.value.filter(project => 
    // Iterate through each filter menu. Project must pass all filtered menus
      Object.keys(activeFilters.value).every(key => // every() returns only if all 3 arrays pass
      // Check if any filter for menu is selected
        activeFilters.value[key].length === 0
          ? true // Pass category
          // Check validity of array
          : Array.isArray(project[key])
            // Check if project meets any of the filters (some)
            ? activeFilters.value[key].some(filter => project[key].includes(filter)) // check for a match between each project and the set filters
            : activeFilters.value[key].includes(project[key]) // single value as string
      )
    )
    // Return all projects
    : projects.value
})
</script>

<template>
  <div>
    <section class="container px-2"> 
      <!-- Portfolio Filter -->
      <!-- Filter Tabs Start -->
      <nav class="navbar border-bottom">
        <div class="list-group list-group-horizontal">
          <i class="list-group-item px-1 icon ti-filter"></i>
          <button v-for="(active, menu) in filterMenus" class="list-group-item list-group-item-action text-capitalize px-2" 
            :class="{
              'list-group-item--active': active,
              'label--filter-active': activeFilters[menu].length > 0
            }" @click="setMenu(menu, active)">
            {{ menu }}&nbsp;<span v-if="activeFilters[menu].length > 0" class="label--filter">
              {{ activeFilters[menu].length }}</span>
          </button>
          <button class="list-group-item small px-2 list-group-item-action text-warning label--clear" type="button" @click="clearAllFilters">
            Clear all
          </button>
        </div>
        <label style="cursor: pointer" data-bs-toggle="modal" data-bs-target="#portfolioModal">View More!</label>
      </nav>
      <!-- Filter Tabs End-->
      <!-- Filter Options Start -->
      <transition-group name="dropdown" tag="div" class="dropdown" :style="dropdown">
        <menu v-for="(options, filter) in filters" v-show="filterMenus[filter]" ref="filterMenu" :key="filter" class="filters">
          <button type="button" v-for="(active, option) in options" @click="setFilter(filter, option)" class="btn btn-secondary filters__btn" :class="{ 'filters__btn--active': active }">
            {{ option }}
          </button>
        </menu>
      </transition-group>
      <!-- Filter Options End -->
      <!-- End Portfolio Filter -->
      <!-- Display Results Start -->
      <div class="container mb-5">
        <transition-group name="project" tag="div" class="row">
          <div class="col-xs-12 col-md-6 col-lg-4 col-xl-3" v-for="project in filteredProjects" :key="project.id">
            <div class="project">
              <div class="portfolio-cover" :style="{ backgroundImage: `url(${project.img})`}">
                <div class="text-light">
                  <p>Languages: {{project.languages}}</p>
                  <h3>{{project.name}}</h3>
                </div>
              </div>
              <div class="portfolio-hover">
                <div class="mb-1">
                  <h4>Summary</h4>
                  <p>{{project.summary}}</p>
                </div>
                <a :href="project.link" class="icon icon--sm radius--circle icon--white-bg">
                  <i class="ti-link"></i>
                </a>
              </div>
            </div>
          </div>
        </transition-group>
      </div>
      <!-- Display Results End -->  
      <!-- Modal -->
      <div class="modal fade" id="portfolioModal" tabindex="-1" aria-labelledby="portfolioModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title" id="portfolioModalLabel">Github Projects</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="close"></button>
            </div>
            <div class="modal-body">
              <p>To see my public repo's, click <a href="https://github.com/QuestMerchant?tab=repositories">here</a></p>
            </div>
          </div>
        </div>
      </div>  
    </section>
  </div>
</template>

<style scoped lang="scss">
.row {
  justify-content: center;
}

.list-group-item {
  border: none;
  background-color: var(--bs-primary-bg);

  &:hover {
    background-color: var(--bs-secondary-bg);
  }
  &--active {
    background-color: var(--bs-tertiary-bg);
    border-radius: 6px;
  }
}

.list {
  position: relative;
  margin-top: 1rem;
  backface-visibility: hidden;
  padding-bottom: 1rem;
  padding-right: 1rem;
}

.label{
  position: relative;

  &::after {
    color: transparent;
    width: 1rem;
    display: inline-block;
  }

  &--clear {
    opacity: 0;
    pointer-events: none;
    transition: opacity 300ms ease-in-out
  }

  &--filter {
    color: white;
    background-color: green;
    width: 1rem;
    height: 1rem;
    font-size: 0.8rem;
    border-radius: 50%;
    display: inline-flex;
    align-items: center; 
    justify-content: center;
  }

  // Change clear opacity when filter is applied. 'filter' must appear before clear in HTML
  &--filter-active ~ &--clear {
    opacity: 1;
    pointer-events: auto;
  }
}

/*------------------
  Filters
------------------*/
.filters {
  padding: 0 1rem;
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;

  &__btn {
    padding: 0.25rem 0.5rem;
    margin-top: 0.5rem;
    margin-right: 0.5rem;
    
    &--active{
      background-color: var(--bs-primary)
    }
  }
}

/*------------------
  Dropdown
------------------*/
.dropdown {
  position: relative;
  overflow: flex;
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
  padding-left: 0px;
  padding-right: 0px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 0.75rem;
  border-radius: 6px;
  box-shadow: 0 0 0 1px #111;
  backface-visibility: hidden; /* Uses hardware acceleration */
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
  text-shadow: 0px 0px 12px rgb(var(--bs-dark-rgb));

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
    background: rgba(var(--bs-info-rgb), 0.85);
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
  top: 50%;
  left: 0;
  right: 0;
  text-align: center;
  padding: 1.875rem;
  opacity: 0;
  -webkit-transform: translate3d(0, 100%, 0);
  transform: translate3d(0, 100%, 0);
  transition-duration: 400ms;
  transition-property: all;
  transition-timing-function: cubic-bezier(0.7, 1, 0.7, 1);
}

.project:hover .portfolio-hover {
  opacity: 1;
  -webkit-transform: translate3d(0, -50%, 0);
  transform: translate3d(0, -50%, 0);
}

.text-light {
  opacity: 1;
  transition: opacity 400ms;
}
.project:hover .text-light {
  opacity: 0;
}
</style>
