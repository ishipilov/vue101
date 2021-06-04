<template>
  <div :id="id">
    <slot :time="time" :input="input" :step="step" :set-now="setNow" :set-time="setTime"></slot>
  </div>
</template>

<script>
export default {
  name: 'VueInputTime',
  data () {
    return {
      id: null,
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
        this.time = arr.map(e => e.toString().length == 2 ? e : 0 + e.toString()).join(":")
      }
    }
  },
	created () { this.id = this.$options.name + this._uid },
  methods: {
    setNow () {
      let withSeconds = false
      let d = new Date()
      if (parseInt(this.input.hours) == d.getHours() && parseInt(this.input.minutes) == d.getMinutes()) {
        withSeconds = true
      }
      this.input.hours = d.getHours()
      this.input.minutes = d.getMinutes()
      if (withSeconds) {
        this.input.seconds = d.getSeconds()
      }
    },
    setTime (value) {
      let arr = value.split(":") || []
      if (arr[0] != undefined) this.input.hours = arr[0]
      if (arr[1] != undefined) this.input.minutes = arr[1]
      if (arr[2] != undefined) this.input.seconds = arr[2]
    }
  }
}
</script>

<style scoped>
</style>
