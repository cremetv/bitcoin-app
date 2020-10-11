<template>
  <div>
    current prices
    <a href="#!" @click.prevent="getCurrentPrices">get prices</a>

    <ul class="prices">
      <li v-for="(values, currency) in prices" :key="currency">
        {{ currency }}: {{ values.last }} {{ values.symbol }}
      </li>
    </ul>

  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup () {
    const prices = ref({})

    const getCurrentPrices = () => {
      fetch('https://blockchain.info/ticker')
        .then(response => response.json())
        .then(data => {
          prices.value = data
        })
    }

    getCurrentPrices()

    return {
      getCurrentPrices,
      prices
    }
  }
}
</script>

<style scoped lang="less">

</style>
