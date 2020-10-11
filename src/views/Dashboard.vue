<template>
  <div class="dashboard">
    <UserOverview username="john" />
    <CurrentPrices />
  </div>
</template>

<script>
import UserOverview from '@/components/UserOverview.vue'
import CurrentPrices from '@/components/CurrentPrices.vue'

export default {
  name: 'Home',
  components: {
    UserOverview,
    CurrentPrices
  }
}
</script>
