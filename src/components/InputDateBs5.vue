<template>
  <vue-input-date>
    <template v-slot="{ id, input, today, view, d, prevMonth, nextMonth, prevYear, nextYear, calendar, changeView, changeDate }">
      <div class="mb-3 row">
        <label :for="id" class="col-sm-2 col-form-label">Date</label>
        <div class="col-sm-10 dropdown">
          <div class="input-group">
            <div class="dropdown">
              <button class="btn btn-link" type="button" :id="id" data-bs-toggle="dropdown" data-bs-auto-close="outside">
                <i class="bi bi-calendar"></i>
              </button>
              <div class="dropdown-menu px-3">
                <div class="d-flex justify-content-between align-items-center">
                  <button type="button" class="btn btn-link" @click.prevent="changeView(prevYear)"><i class="bi bi-chevron-double-left"></i></button>
                  <button type="button" class="btn btn-link" @click.prevent="changeView(prevMonth)"><i class="bi bi-chevron-left"></i></button>
                  <button type="button" class="btn btn-link" @click.prevent="changeView(today)"><i class="bi bi-circle"></i></button>
                  <button type="button" class="btn btn-link" @click.prevent="changeView(nextMonth)"><i class="bi bi-chevron-right"></i></button>
                  <button type="button" class="btn btn-link" @click.prevent="changeView(nextYear)"><i class="bi bi-chevron-double-right"></i></button>
                </div>
                <h6 class="dropdown-header d-flex justify-content-between">
                  <div>{{ view.getFullYear() }}</div>
                  <div>{{ ['Январь','Февраль','Март','Апрель','Март','Июнь','Июль','Август','Сентябрь','Октябрь','Ноябрь','Декабрь'][view.getMonth()] }}</div>
                </h6>
                <div class="table-responsive">
                  <table class="table table-bordered table-sm mb-0" style="user-select: none;">
                    <thead class="thead-light">
                      <tr align="center">
                        <th v-for="v, k in ['пн','вт','ср','чт','пт','сб','вс']" :key="k">{{ v }}</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr align="center" v-for="(week, w) in calendar" :key="w">
                        <td v-for="(cell, c) in week" :key="c" :class="[ cell.getMonth() != view.getMonth() ? 'bg-light text-muted' : '', cell.getTime() == today.getTime() ? 'text-info font-weight-bold' : '', d && (cell.getFullYear() == d.getFullYear() && cell.getMonth() == d.getMonth() && cell.getDate() == d.getDate()) ? 'bg-primary text-white' : '' ]" style="cursor: pointer;" @click.prevent="changeDate(cell)">
                          {{ cell.getDate() }}
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <a class="dropdown-item text-center" href="#" @click.prevent="changeDate(today)">Set today</a>
              </div>
            </div>
            <input v-model="input.date" type="date" class="form-control" name="date">
          </div>
        </div>
      </div>
    </template>
  </vue-input-date>
</template>

<script>
import VueBaseComponent from './BaseComponent.vue'
import VueInputDate from './InputDate.vue'

export default {
  name: 'VueInputDateBs5',
  extends: VueBaseComponent,
  components: {
    VueInputDate
  }
}
</script>

<style scoped>
</style>
