<template>
  <DaySelection />
</template>

<script lang="ts">
import Vue from 'vue'
import DaySelection from '~/components/DaySelection.vue'

export default Vue.extend({
  name: 'DaySelectionPage',
  components: { DaySelection }
})
</script>
