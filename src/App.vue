<template>
  <div id="app">
    <main>
      <Navbar/>
      <router-view/>
    </main>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar'

export default {
  components: {
    Navbar
  },
  data: () => ({
    windowHeight: window.innerHeight
  }),
  mounted () {
    this.realVh()
    window.addEventListener('resize', this.realVh)
  },
  methods: {
    realVh () {
      const vh = window.innerHeight * 0.01
      document.querySelector('#app').style.height = vh * 100 + 'px'
    }
  }
}
</script>

<style lang="scss">

</style>
