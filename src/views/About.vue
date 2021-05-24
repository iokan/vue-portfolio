<template>
  <section class="about">
    <div class="container">
      <h2>О себе</h2>
      <Timeline/>
    </div>
  </section>
</template>

<script>
import Timeline from '../components/Timeline'

export default {
  name: 'About',
  components: {
    Timeline
  }
}
</script>

<style scoped>

</style>
