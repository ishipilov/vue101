<template>
  <div :id="id">
    <slot :time="time" :input="input" :step="step" :set-now="setNow" :set-time="setTime"></slot>
  </div>
</template>

<script>
import VueBaseComponent from '../components/BaseComponent.vue'

export default {
  name: 'VueInputTime',
  extends: VueBaseComponent,
  data () {
    return {
      time: "",
      input: {
        hours: null,
        minutes: null,
        seconds: null
      }
    }
  },
  computed: {
    step () { return this.input.seconds ? 1 : 60 }
  },
  watch: {
    input: {
      deep: true,
      immediate: true,
      handler (val) {
        if (val.hours && val.minutes) {
          let arr = [val.hours, val.minutes]
          if (val.seconds) arr.push(val.seconds)
          this.time = arr.map(e => e.length == 2 ? e : "0" + e).join(":")
        } else {
          this.time = ""
        }
      }
    }
  },
  methods: {
    setNow () {
      let d = new Date()
      this.input.hours = d.getHours().toString()
      this.input.minutes = d.getMinutes().toString()
      if (this.input.seconds) this.input.seconds = d.getSeconds().toString()
    },
    setTime (value = "") {
      let arr = value.split(":")
      this.input.hours = arr[0] || null
      this.input.minutes = arr[1] || null
      this.input.seconds = arr[2] || null
    }
  }
}
</script>

<style scoped>
</style>
