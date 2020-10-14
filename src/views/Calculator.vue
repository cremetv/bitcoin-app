<template>
  <div class="calculator">

    <div class="display">
      <div class="input">
        <div class="prefix"></div>
        <input ref="input" v-model="inputValue" name="inputValue">
        <div class="currency">
          <div class="custom-select">
            <select name="currency" id="currency" v-model="currency" @change="changeCurrency($event.target.value)">
              <option v-for="(values, currency, i) in currencies" :key="i" :value="currency">{{ currency }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="output">
        <div class="prefix">=</div>
        <div class="value">{{ outputValue }}</div>
        <div class="suffix">BTC</div>
      </div>
    </div>

    <div class="keypad">
      <div class="key" v-for="key in keypadKeys" :key="key.key">
        <a href="#!" @click.prevent="pressKey(key.key)"><span v-html="key.content"></span></a>
      </div>
    </div>

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

    const keypadKeys = [
      {
        key: '1',
        content: '1'
      }, {
        key: '2',
        content: '2'
      }, {
        key: '3',
        content: '3'
      }, {
        key: '4',
        content: '4'
      }, {
        key: '5',
        content: '5'
      }, {
        key: '6',
        content: '6'
      }, {
        key: '7',
        content: '7'
      }, {
        key: '8',
        content: '8'
      }, {
        key: '9',
        content: '9'
      }, {
        key: '.',
        content: '.'
      }, {
        key: '0',
        content: '0'
      }, {
        key: 'del',
        content: '<svg width="35" height="35" viewBox="0 0 35 35" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M35 16.25H4.7875L9.2625 11.7625L7.5 10L0 17.5L7.5 25L9.2625 23.2375L4.7875 18.75H35V16.25Z" fill="#202225"/></svg>'
      }
    ]

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

    const changeCurrency = (val) => {
      currency.value = val
      calculate()
    }

    const calculate = () => {
      if (inputValue.value == null || inputValue.value === '') return
      fetch(`https://blockchain.info/tobtc?currency=${currency.value}&value=${inputValue.value}`)
        .then(response => response.json())
        .then(data => {
          outputValue.value = data
        })
    }

    // handle keypad inputs
    const pressKey = (key) => {
      if (key === 'del') {
        // remove last character
        inputValue.value = inputValue.value.substring(0, inputValue.value.length - 1)
      } else {
        // add a 0 if the input value is empty
        if (inputValue.value === null || inputValue.value === '') {
          if (key === '.') {
            inputValue.value = `0${key}`
            return
          }
        }
        // prevent multiple dots
        if (key === '.' && inputValue.value.match(/\./g)) return

        // add pressed key
        inputValue.value === null
          ? inputValue.value = key
          : inputValue.value += key
      }

      calculate()
    }

    onMounted(() => {
      // input.value.focus()
      // populate the select element
      getCurrencies()
    })

    return {
      inputValue,
      outputValue,
      currencies,
      currency,
      calculate,
      changeCurrency,
      input,
      pressKey,
      keypadKeys
    }
  }
}
</script>

<style scoped lang="less">
@import '../_styles/_mixins.less';
.display {
  width: 100%;
  padding: 3.5rem .5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #FAF8F2;

  position: Sticky;
  top: 0;
  z-index: 1;

  .input {
    display: flex;
    flex-direction: row;
    max-width: 500px;

    input {
      border: none;
      outline: none;
      background: transparent;
      font-size: 2.5rem;
      color: #36393F;
      // text-align: center;
      text-align: right;
      .fw(--fw-semiBold);
      flex: 1;
      width: 100%;
    }

    .currency {
      color: #A2907A;
      margin-left: 1rem;
    }
  }

  .output {
    display: flex;
    flex-direction: row;
    margin-top: 1rem;
    color: #36393F;
    font-size: 2.5rem;
    .fw(--fw-semiBold);

    .value {

    }

    .prefix {
      margin-right: .5em;
      opacity: .4;
    }

    .suffix {
      margin-left: .5em;
    }
  }
}

.keypad {
  max-width: 500px;
  margin: 0 auto;
  color: var(--text-normal);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  background: var(--background-primary);

  .key {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;

    &:before {
      display: block;
      content: "";
      width: 100%;
      padding-top: (1 / 1) * 100%;
    }

    a {
      position: absolute;
      text-decoration: none;
      color: var(--text-normal);
      .fw(--fw-regular);
      font-size: 2rem;

      width: 4rem;
      height: 4rem;
      border-radius: 50%;
      background: var(--background-primary);

      transition: background .25s ease;

      display: flex;
      justify-content: center;
      align-items: center;

      &:active {
        background: var(--background-secondary);
      }
    }

    a span {
      display: flex;
    }
  }
}
</style>
