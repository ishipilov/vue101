<template>
  <div>
    <slot :id="$options.name + _uid"
          :input="input"
          :clock="clock"
          :step="step"
          :set-now="setNow"
    ></slot>
  </div>
</template>

<script>
import VueBaseComponent from '../components/BaseComponent.vue'

export default {
  name: 'VueInputTime',
  extends: VueBaseComponent,
  props: {
    time: { type: String, default: "" }
  },
  data () {
    return {
      input: {
        time: this.time
      },
      clock: {
        hours: "",
        minutes: "",
        seconds: ""
      }
    }
  },
  computed: {
    step () { return this.clock.seconds ? 1 : 60 }
  },
  watch: {
    'input.time': {
      immediate: true,
      handler (val) {
        let arr = val.split(":")
        this.clock.hours = arr[0] || ""
        this.clock.minutes = arr[1] || ""
        this.clock.seconds = arr[2] || ""
      }
    },
    clock: {
      deep: true,
      immediate: true,
      handler (val) {
        if (val.hours && val.minutes) {
          let arr = [val.hours, val.minutes]
          if (val.seconds) arr.push(val.seconds)
          this.input.time = arr.map(e => this.prependZero(e.toString())).join(":")
        } else {
          this.input.time = ""
        }
      }
    }
  },
  methods: {
    setNow () {
      let d = new Date()
      let hours = d.getHours().toString()
      let minutes = d.getMinutes().toString()
      if (this.clock.seconds || (this.clock.hours == hours && this.clock.minutes == minutes)) this.clock.seconds = d.getSeconds().toString()
      this.clock.minutes = minutes
      this.clock.hours = hours
    },
    prependZero (str, len = 2) {
      let substr = ""
      if (str.length < len) substr = "0".repeat(len - str.length)
      return substr + str
    }
  }
}
</script>

<style scoped>
</style>
