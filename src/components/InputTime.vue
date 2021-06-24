<template>
  <div>
    <slot :id="uid"
          :input="input"
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
        time: this.time,
        hours: "",
        minutes: "",
        seconds: ""
      }
    }
  },
  computed: {
    step () { return this.input.seconds ? 1 : 60 },
    clock () {
      return {
        hours: this.input.hours,
        minutes: this.input.minutes,
        seconds: this.input.seconds
      }
    }
  },
  watch: {
    'input.time': {
      immediate: true,
      handler (val) {
        let arr = val.split(":")
        this.input.hours = arr[0] || ""
        this.input.minutes = arr[1] || ""
        this.input.seconds = arr[2] || ""
      }
    },
    clock: {
      deep: true,
      immediate: true,
      handler (val) {
        if (val.hours && !val.minutes) this.input.minutes = "00"
        if (val.hours && val.hours.length < 2) this.input.hours = this.prependZero(val.hours)
        if (val.minutes && val.minutes.length < 2) this.input.minutes = this.prependZero(val.minutes)
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
      let hours = this.prependZero(d.getHours().toString())
      let minutes = this.prependZero(d.getMinutes().toString())
      if (this.input.seconds || (this.input.hours == hours && this.input.minutes == minutes)) {
        this.input.seconds = this.prependZero(d.getSeconds().toString())
      }
      this.input.minutes = minutes
      this.input.hours = hours
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
