<template>
  <div v-if="loading" class="loading-page">
    <p>Loading...</p>
  </div>
</template>

<script>
export default {
  data () {
    return { loading: false }
  },
  methods: {
    start () { this.loading = true },
    finish () { this.loading = false },
  }
}
</script>

<style scoped>
.loading-page {
  position: fixed;
  display: grid;
  place-content: center;
  position: absolute;
  width: 100vw;
  height: 100vh;

}
</style>