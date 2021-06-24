<template>
  <div class="container">

    <vue-input-date-bs5 />

    <vue-input-time-bs5 />

    <vue-input-datetime-bs5 />

  </div>
</template>

<script>
import VueInputDateBs5 from './components/InputDateBs5.vue'
import VueInputTimeBs5 from './components/InputTimeBs5.vue'
import VueInputDatetimeBs5 from './components/InputDatetimeBs5.vue'

export default {
  name: 'Demo',
  components: {
    VueInputDateBs5,
    VueInputTimeBs5,
    VueInputDatetimeBs5
  }
}
</script>

<style scoped>
</style>
