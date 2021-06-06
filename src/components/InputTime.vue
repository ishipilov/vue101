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
        hours: "0",
        minutes: "0",
        seconds: "0"
      }
    }
  },
  computed: {
    step () { return parseInt(this.input.seconds) > 0 ? 1 : 60 }
  },
  watch: {
    input: {
      deep: true,
      immediate: true,
      handler (val) {
        let arr = [val.hours, val.minutes]
        if (this.step == 1) {
          arr.push(val.seconds)
        }
        this.time = arr.map(e => e.length == 2 ? e : "0" + e).join(":")
      }
    }
  },
  methods: {
    setNow () {
      let withSeconds = false
      let d = new Date()
      if (parseInt(this.input.hours) == d.getHours() && parseInt(this.input.minutes) == d.getMinutes()) {
        withSeconds = true
      }
      this.input.hours = d.getHours().toString()
      this.input.minutes = d.getMinutes().toString()
      if (withSeconds) {
        this.input.seconds = d.getSeconds().toString()
      }
    },
    setTime (value = "") {
      let str = value.split(":")
      let arr = ["0", "0", "0"].map((v, k) => str[k] && str[k].length > 0 ? str[k] : v)
      this.input.hours = arr[0]
      this.input.minutes = arr[1]
      this.input.seconds = arr[2]
    }
  }
}
</script>

<style scoped>
</style>
