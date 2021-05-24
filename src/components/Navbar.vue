<template>
  <nav class="navbar" :class="{navbar_open:btnCloseNav == true}">
      <router-link
        v-for="link in links"
        :key="link.url"
        :to="link.url"
        :exact="link.exact"
        @click="btnCloseNav = false"
      >{{link.title}}</router-link>
    <button @click="btnCloseNav = !btnCloseNav" class="burger">
      <transition name="burger">
        <div v-if="!btnCloseNav" class="burger__open" key="open">
          <span>M</span>
          <span>E</span>
          <span>N</span>
          <span>U</span>
        </div>
        <div v-else class="burger__close" key="close">
          <span></span><span></span>
        </div>
      </transition>
    </button>
  </nav>
</template>

<script>
export default {
  data: () => ({
    links: [
      { title: 'Домой', url: '/', exact: true },
      { title: 'О себе', url: '/about' },
      { title: 'Навыки', url: '/skills' },
      { title: 'Опыт', url: '/experience' },
      { title: 'Проекты', url: '/project' }
    ],
    btnCloseNav: false
  }),
  watch: {
    $route (to, from) {
      if (window.innerWidth < 767) {
        this.btnCloseNav = false
      }
    }
  }
}
</script>

<style scoped>

</style>
