<template>
  <div>
    <slot :id="uid"
          :input="input"
          :today="today"
          :view="view"
          :d="d"
          :prev-month="prevMonth"
          :next-month="nextMonth"
          :prev-year="prevYear"
          :next-year="nextYear"
          :calendar="calendar"
          :change-view="changeView"
          :change-date="changeDate"
    ></slot>
  </div>
</template>

<script>
import VueBaseComponent from '../components/BaseComponent.vue'

export default {
  name: 'VueInputDate',
  extends: VueBaseComponent,
  props: {
    date: { type: String, default: "" }
  },
  data () {
    return {
      input: {
        date: this.date
      },
      today: this.todayDate(),
      view: this.todayDate()
    }
  },
  computed: {
    d () {
      if (this.input.date.length == 0) return null
      let date = this.input.date ? new Date(this.input.date) : new Date()
      date.setHours(0)
      date.setMinutes(0)
      date.setSeconds(0)
      date.setMilliseconds(0)
      return date
    },
    prevMonth () {
      let d = new Date(this.view.getFullYear(), this.view.getMonth(), 1)
      let timestamp = d.setMonth(this.view.getMonth() - 1)
      d = new Date(timestamp)
      let lastDate = new Date(d.getFullYear(), d.getMonth() + 1, 0)
      if (this.view.getDate() > lastDate.getDate()) {
        d.setDate(lastDate.getDate())
      } else {
        d.setDate(this.view.getDate())
      }
      return d
    },
    nextMonth () {
      let d = new Date(this.view.getFullYear(), this.view.getMonth(), 1)
      let timestamp = d.setMonth(this.view.getMonth() + 1)
      d = new Date(timestamp)
      let lastDate = new Date(d.getFullYear(), d.getMonth() + 1, 0)
      if (this.view.getDate() > lastDate.getDate()) {
        d.setDate(lastDate.getDate())
      } else {
        d.setDate(this.view.getDate())
      }
      return d
    },
    prevYear () {
      let d = new Date(this.view.getFullYear(), this.view.getMonth(), 1)
      let timestamp = d.setFullYear(this.view.getFullYear() - 1)
      d = new Date(timestamp)
      let lastDate = new Date(d.getFullYear(), d.getMonth() + 1, 0)
      if (this.view.getDate() > lastDate.getDate()) {
        d.setDate(lastDate.getDate())
      } else {
        d.setDate(this.view.getDate())
      }
      return d
    },
    nextYear () {
      let d = new Date(this.view.getFullYear(), this.view.getMonth(), 1)
      let timestamp = d.setFullYear(this.view.getFullYear() + 1)
      d = new Date(timestamp)
      let lastDate = new Date(d.getFullYear(), d.getMonth() + 1, 0)
      if (this.view.getDate() > lastDate.getDate()) {
        d.setDate(lastDate.getDate())
      } else {
        d.setDate(this.view.getDate())
      }
      return d
    },
    calendar () {
      let year = this.view.getFullYear()
      let month = this.view.getMonth()

      let prevLastDate = new Date(year, month, 0)
      let prepend = this.range(prevLastDate.getDate(), 1).slice(prevLastDate.getDate() - (prevLastDate.getDay() || 7))
      prepend = prepend.map(e => e ? new Date(year, month - 1, e) : e)
      
      let lastDate = new Date(year, month + 1, 0)
      let calendar = this.range(lastDate.getDate(), 1)
      calendar = calendar.map(e => e ? new Date(year, month, e) : e)

      let append = this.range(7 - lastDate.getDay(), 1)
      append = append.map(e => e ? new Date(year, month + 1, e) : e)

      if (this.batch([...prepend, ...calendar, ...append], 7).length < 6) {
        let append2 = this.range(7 + append.length, 1).slice(append.length)
        append2 = append2.map(e => e ? new Date(year, month + 1, e) : e)

        calendar = [...prepend, ...calendar, ...append, ...append2]
      } else {
        calendar = [...prepend, ...calendar, ...append]
      }

      return this.batch(calendar, 7)
    }
  },
  watch: {
    d: {
      immediate: true,
      handler () {
        if (this.d) {
          let d = this.d.getFullYear() + "-" + this.d.getMonth()
          let v = this.view.getFullYear() + "-" + this.view.getMonth()
          if (d != v) this.view = this.d
        }
      }
    }
  },
  methods: {
    changeView (date) { this.view = date },
    changeDate (date) { this.input.date = this.datetimeString(date, 1) },
    todayDate () {
      let date = new Date()
      date.setHours(0)
      date.setMinutes(0)
      date.setSeconds(0)
      date.setMilliseconds(0)
      return date
    },
    datetimeString (date, index = 0) {
      let m = date.getMonth() + 1
      let d = date.getDate()
      let h = date.getHours()
      let i = date.getMinutes()
      let s = date.getSeconds()
      let dateStr = [date.getFullYear(), m, d].map(e => this.prependZero(e.toString())).join("-")
      let timeStr = [h, i, s].map(e => this.prependZero(e.toString())).join(":")
      let datetimeStr = [dateStr, timeStr].join(" ")
      return [datetimeStr, dateStr, timeStr][index]
    },
    prependZero (str, len = 2) {
      let substr = ""
      if (str.length < len) substr = "0".repeat(len - str.length)
      return substr + str
    },
    batch (arr, n) {
      let result = []
      do {
        result.push(arr.splice(0, n))
      } while (arr.length > 0)
      return result
    },
    range (n, i = 0) { return Array.from(new Array(n), (v, k) => k + i) }
  }
}
</script>

<style scoped>
</style>
