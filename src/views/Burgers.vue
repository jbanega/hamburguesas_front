<template>
  <div>
    <h1>Hamburguesas</h1>
  </div>
  <div class="col-md-12">
    <burgers-overview :burgers="burgers" />
  </div>
</template>

<script>
import BurgersOverview from '@/components/BurgersOverview.vue'

export default {
  name: 'Burgers',
  components: {
    BurgersOverview,
  },
}
</script>

<style>
</style>