<template>
  <vue-input-time>
    <template v-slot="{ id, input, step, setNow }">
      <div class="mb-3 row">
        <label :for="id" class="col-sm-2 col-form-label">Time</label>
        <div class="col-sm-10">
          <div class="input-group">
            <div class="dropdown">
              <button class="btn btn-link" type="button" :id="id" data-bs-toggle="dropdown" data-bs-auto-close="outside">
                <i class="bi bi-clock"></i>
              </button>
              <div class="dropdown-menu px-3">
                <label :for="id + '_hours'" class="form-label d-flex justify-content-between">
                  <div>Hours</div>
                  <div>{{ input.hours }}</div>
                </label>
                <input v-model="input.hours" type="range" class="form-range" min="0" max="23" :id="id + '_hours'">
                <label :for="id + '_minutes'" class="form-label d-flex justify-content-between">
                  <div>Minutes</div>
                  <div>{{ input.minutes }}</div>
                </label>
                <input v-model="input.minutes" type="range" class="form-range" min="0" max="59" :id="id + '_minutes'">
                <label :for="id + '_seconds'" class="form-label d-flex justify-content-between">
                  <div>Seconds</div>
                  <div>{{ input.seconds }}</div>
                </label>
                <input v-model="input.seconds" type="range" class="form-range" min="0" max="59" :id="id + '_seconds'">
                <a class="dropdown-item text-center" href="#" @click.prevent="setNow">Set now</a>
              </div>
            </div>
            <input v-model="input.time" type="time" class="form-control" name="time" :step="step">
          </div>
        </div>
      </div>
    </template>
  </vue-input-time>
</template>

<script>
import VueBaseComponent from '../components/BaseComponent.vue'
import VueInputTime from '../components/InputTime.vue'

export default {
  name: 'VueInputTimeBs5',
  extends: VueBaseComponent,
  components: {
    VueInputTime
  }
}
</script>

<style scoped>
</style>
