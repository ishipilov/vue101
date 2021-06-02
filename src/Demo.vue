<template>
  <div>
    <vue-input-date>
      <template v-slot="{input, setInput, date, setDate, prev, today, next, table}">
        <input type="date" :value="input" @change="setInput($event.target.value)">
        <div>
          <button @click="setDate(prev)">prev Month</button>
          <button @click="setDate(today)">Today</button>
          <button @click="setDate(next)">next Month</button>
        </div>
        <table>
          <thead>
            <tr>
              <th v-for="v, k in ['пн','вт','ср','чт','пт','сб','вс']" :key="k">{{ v }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(row, rowKey) in table" :key="rowKey">
              <td v-for="(cell, cellKey) in row" :key="cellKey" :title="cell" @click="setDate(cell)">
                {{ cell.getDate() }}
                <span v-if="cell.getTime() == today.getTime()">!</span>
                <span v-if="cell.getTime() == date.getTime() && input.length > 0">*</span>
              </td>
            </tr>
          </tbody>
        </table>
      </template>
    </vue-input-date>
  </div>
</template>

<script>
import VueInputDate from './components/InputDate.vue'

export default {
  name: 'Demo',
  components: {
    VueInputDate
  }
}
</script>

<style scoped>
</style>
