<template>
  <PackageCard />
</template>

<script lang="ts">
import Vue from 'vue'
import PackageCard from '~/components/PackageCard.vue'

export default Vue.extend({
    name: "IndexPage",
    components: { PackageCard }
})
</script>
