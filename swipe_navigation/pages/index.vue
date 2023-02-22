<script lang="ts">
import Vue from 'vue'
import ContentAboutVue from '../components/contentAbout.vue'
import ContentHomeVue from '../components/contentHome.vue'

export default Vue.extend({
  components: {
    ContentAboutVue,
    ContentHomeVue
  },
  data() {
    return{
      currentTab: 'ContentHomeVue',
      tabs: [
      'ContentHomeVue',
      'ContentAboutVue'
      ],
      touch: {
        startX: 0,
        endX: 0
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener('scroll', this.onScroll)
      window.addEventListener('touchstart', (event) => this.onTouchStart(event))
      window.addEventListener('touchmove', (event) => this.onTouchMove(event))
      window.addEventListener('touchend', () => this.onTouchEnd())
    })
  },
  methods: {
    changeTab(tabName: string) {
      this.currentTab = tabName
    },
    onTouchStart(event: any) {
      this.touch.startX = event.touches[0].clientX
      this.touch.endX = 0
    },
    onTouchMove(event: any) {
      this.touch.endX = event.touches[0].clientX
    },
    onTouchEnd() {
      if (
        Math.abs(this.touch.endX - this.touch.startX) > 100 &&
        this.touch.endX !== 0
      ) {
        if (this.touch.endX < this.touch.startX) {
          this.switchToNextTab()
        } else {
          this.switchToPreviusTab()
        }
      }
    },
    switchToNextTab() {
      const index = this.tabs.indexOf(this.currentTab)
      if (index < this.tabs.length - 1) {
        this.currentTab = this.tabs[index + 1]
      }
    },
    switchToPreviusTab() {
      const index = this.tabs.indexOf(this.currentTab)
      if (index !== 0) {
        this.currentTab = this.tabs[index - 1]
      }
    },
  }
})
</script>

<template>
  <div class="container">
    <header>
      <!-- the navbar will be insert here -->
    </header>
    <main>
      <component :is="currentTab"></component>
    </main>
  </div>
</template>