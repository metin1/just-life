<template>
  <div>
    <div>About page</div>
    <span class="link" @click="goToFinal"> click here </span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
  layout: 'about',
  scrollToTop: true,
  loading: false,
  mounted() {
    setTimeout(() => {
      this.$nuxt.$loading.finish()
    }, 5000)
  },
  methods: {
    goToFinal() {
      this.$nuxt.$loading.start()
      setTimeout(() => {
        this.$router.push('/day-selection')
      }, 5000)
    },
  },
})
</script>
