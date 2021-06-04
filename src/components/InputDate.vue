<template>
  <div :id="id">
    <slot :input="input" :set-input="setInput" :date="date" :set-date="setDate" :prev="prevMonth" :today="today" :next="nextMonth" :table="table"></slot>
  </div>
</template>

<script>
export default {
  name: 'VueInputDate',
  data () {
    return {
      id: null,
      input: ""
    }
  },
  computed: {
    date () { return this.inputToDate(this.input) },
    today () { return this.inputToDate("") },
    prevMonth () {
      let d = new Date(this.date.getFullYear(), this.date.getMonth(), 1)
      let timestamp = d.setMonth(this.date.getMonth() - 1)
      d = new Date(timestamp)
      let lastDate = new Date(d.getFullYear(), d.getMonth() + 1, 0)
      if (this.date.getDate() > lastDate.getDate()) {
        d.setDate(lastDate.getDate())
      } else {
        d.setDate(this.date.getDate())
      }
      return d
    },
    nextMonth () {
      let d = new Date(this.date.getFullYear(), this.date.getMonth(), 1)
      let timestamp = d.setMonth(this.date.getMonth() + 1)
      d = new Date(timestamp)
      let lastDate = new Date(d.getFullYear(), d.getMonth() + 1, 0)
      if (this.date.getDate() > lastDate.getDate()) {
        d.setDate(lastDate.getDate())
      } else {
        d.setDate(this.date.getDate())
      }
      return d
    },
    table () {
      let year = this.date.getFullYear()
      let month = this.date.getMonth()

      let prevLastDate = new Date(year, month, 0)
      let prepend = this.range(prevLastDate.getDate(), 1).slice(prevLastDate.getDate() - (prevLastDate.getDay() || 7))
      prepend = prepend.map(e => e ? new Date(year, month - 1, e) : e)
      
      let lastDate = new Date(year, month + 1, 0)
      let table = this.range(lastDate.getDate(), 1)
      table = table.map(e => e ? new Date(year, month, e) : e)

      let append = this.range(7 - lastDate.getDay(), 1)
      append = append.map(e => e ? new Date(year, month + 1, e) : e)

      if (this.batch([...prepend, ...table, ...append], 7).length < 6) {
        let append2 = this.range(7 + append.length, 1).slice(append.length)
        append2 = append2.map(e => e ? new Date(year, month + 1, e) : e)

        table = [...prepend, ...table, ...append, ...append2]
      } else {
        table = [...prepend, ...table, ...append]
      }

      return this.batch(table, 7)
    }
  },
	created () { this.id = this.$options.name + this._uid },
  methods: {
    range (n, i = 0) { return Array.from(new Array(n), (v, k) => k + i) },
    batch (arr, n) {
      let result = []
      do {
        result.push(arr.splice(0, n))
      } while (arr.length > 0)
      return result
    },
    inputToDate (str = "") {
      let re = new RegExp(/^\d{4}-\d{1,2}-\d{1,2}$/) // YYYY-MM-DD or YYYY-M-D
      let arr = str.match(re) ? str.split("-").map(e => parseInt(e)) : []
      let date = new Date()
      if (arr[0] != undefined) date.setFullYear(arr[0])
      if (arr[1] != undefined) date.setMonth(arr[1] - 1)
      if (arr[2] != undefined) date.setDate(arr[2])
      date.setHours(0)
      date.setMinutes(0)
      date.setSeconds(0)
      date.setMilliseconds(0)
      return date
    },
    dateToInput (date) {
      let m = date.getMonth() + 1
      m = m.toString().length == 2 ? m : 0 + m.toString()
      let d = date.getDate()
      d = d.toString().length == 2 ? d : 0 + d.toString()
      return [date.getFullYear(), m, d].join("-")
    },
    setDate (date) { this.input = this.dateToInput(date) },
    setInput (value) { this.input = value }
  }
}
</script>

<style scoped>
</style>
