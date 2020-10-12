<template>
  <div class="details">
    <h1>This is the details page</h1>
    <div>
      <h3>Marktkapitalisierung</h3>
      <div class="value">
        {{ stats.marketcap }} $
      </div>
    </div>

    <div>
      <h3>Bitcoins im Umlauf</h3>
      <div class="value">
        {{ stats.totalbc }}
      </div>
    </div>

    <div>
      <h3>Transaktionen</h3>
      <div class="value">
        {{ stats.n_tx }}
      </div>
    </div>

    <div>
      <h3>gesendete Bitcoins</h3>
      <div class="value">
        {{ stats.estimated_btc_sent }}
      </div>
    </div>

    <div>
      <h3>Hashrate</h3>
      <div class="value">
        {{ stats.hash_rate }} GH/s
      </div>
    </div>

    <div>
      <h3>Schwierigkeitsgrad</h3>
      <div class="value">
        {{ stats.difficulty }}
      </div>
    </div>

    <div>stand: {{ stats.timestamp }}</div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup () {
    const stats = ref({})

    const formatNumber = (x) => {
      const parts = x.toString().split('.')
      parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ',')
      return parts.join('.')
    }

    const getStats = () => {
      fetch('https://api.blockchain.info/stats')
        .then(response => response.json())
        .then(data => {
          // TODO: api is serving a way too high number?! maybe? idk -> check
          const totalbc = data.totalbc / 100000000
          stats.value.marketcap = formatNumber(totalbc * data.market_price_usd)
          stats.value.totalbc = formatNumber(totalbc)
          stats.value.n_tx = formatNumber(data.n_tx)
          stats.value.estimated_btc_sent = formatNumber(data.estimated_btc_sent)
          stats.value.hash_rate = formatNumber(data.hash_rate)
          stats.value.difficulty = formatNumber(data.difficulty)

          // convert timestamp to human readable string
          const d = new Date(data.timestamp)
          const hours = d.getHours()
          const minutes = '0' + d.getMinutes()
          const seconds = '0' + d.getSeconds()
          stats.value.timestamp = `${d.getDate()}.${d.getMonth() + 1}.${d.getFullYear()} ${hours}:${minutes.substr(-2)}:${seconds.substr(-2)}`
        })
    }

    onMounted(() => {
      getStats()
    })

    return {
      getStats,
      stats
    }
  }
}
</script>
