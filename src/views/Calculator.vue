<template>
  <div class="calculator">
    <h1>This is the calculator page</h1>

    <form @submit.prevent="calculate">
      <label>Input Value</label>
      <input ref="input" v-model="inputValue" name="inputValue">
      <select v-model="currency" @change="changeCurrency($event.target.value)">
        <!-- <option selected="selected">USD</option>
        <option>EUR</option> -->
        <option v-for="(values, currency) in currencies" :key="currency">{{ currency }}</option>
      </select>
      <button>calculate</button>
    </form>

    <div class="output">{{ outputValue }} {{ currency }}</div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {

  setup () {
    const inputValue = ref(null)
    const outputValue = ref(0)
    const currencies = ref({})
    // default currency
    const currency = ref('EUR')

    // access the input element
    const input = ref(null)

    // get all currencies from the api
    const getCurrencies = () => {
      fetch('https://blockchain.info/ticker')
        .then(response => response.json())
        .then(data => {
          currencies.value = data
        })
    }
    // populate the select element
    getCurrencies()

    const changeCurrency = (val) => {
      currency.value = val
    }

    const calculate = () => {
      fetch(`https://blockchain.info/tobtc?currency=${currency.value}&value=${parseFloat(inputValue.value)}`)
        .then(response => response.json())
        .then(data => {
          outputValue.value = data
        })
    }

    onMounted(() => {
      input.value.focus()
    })

    return {
      inputValue,
      outputValue,
      currencies,
      currency,
      calculate,
      changeCurrency,
      input
    }
  }
}
</script>
