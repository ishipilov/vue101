<template>
  <div></div>
</template>

<script>
export default {
  name: 'VueBaseComponent',
  props: {
    isWaiting: { type: Boolean, dafault: false }
  },
  data () {
    return {
      id: null,
      ready: !this.isWaiting,
      waiting: this.isWaiting
    }
  },
  watch: {
    waiting (val) {
      if (val) {
        this.ready = false
      }
    }
  },
	created () { this.id = this.$options.name + this._uid },
}
</script>

<style scoped>
</style>
