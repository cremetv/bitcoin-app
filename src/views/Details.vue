<template>
  <div class="details">
    <h1>Details</h1>

    <ul class="stats">
      <li v-for="stat in stats" :key="stat.title" class="card">
        <div class="title">{{ stat.title }}</div>
        <div class="value">{{ stat.value }} <span class="suffix">{{ stat.suffix }}</span></div>
      </li>
    </ul>

    <div class="updated">Stand: {{updated }}</div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup () {
    const updated = ref('')
    const stats = ref({
      marketcap: {
        title: 'Marktkapitalisierung',
        value: 0
      },
      totalbc: {
        title: 'Bitcoins im Umlauf',
        value: 0
      },
      n_tx: {
        title: 'Transaktionen',
        value: 0
      },
      estimated_btc_sent: {
        title: 'gesendete Bitcoins',
        value: 0
      },
      hash_rate: {
        title: 'Hashrate',
        suffix: 'GH/s',
        value: 0
      },
      difficulty: {
        title: 'Schwierigkeitsgrad',
        value: 0
      }
    })

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
          stats.value.marketcap.value = formatNumber(totalbc * data.market_price_usd)
          stats.value.totalbc.value = formatNumber(totalbc)
          stats.value.n_tx.value = formatNumber(data.n_tx)
          stats.value.estimated_btc_sent.value = formatNumber(data.estimated_btc_sent)
          stats.value.hash_rate.value = formatNumber(data.hash_rate)
          stats.value.difficulty.value = formatNumber(data.difficulty)

          // convert timestamp to human readable string
          const d = new Date(data.timestamp)
          const hours = d.getHours()
          const minutes = '0' + d.getMinutes()
          const seconds = '0' + d.getSeconds()
          updated.value = `${d.getDate()}.${d.getMonth() + 1}.${d.getFullYear()} ${hours}:${minutes.substr(-2)}:${seconds.substr(-2)}`
        })
    }

    onMounted(() => {
      getStats()
    })

    return {
      getStats,
      stats,
      updated
    }
  }
}
</script>

<style scoped lang="less">
@import '../_styles/_mixins.less';
@import '../_styles/_cards.less';

.details {
  padding: 1rem;
  max-width: 900px;
  margin: 0 auto;
}

.card {

  .title {
    font-size: 1.5rem;
    color: var(--text-muted);
    .fw(--fw-semiBold);
    margin-bottom: 1rem;
  }

  .value {
    align-self: flex-end;
    font-size: 1.5rem;
    .fw(--fw-semiBold);
  }

  .suffix {
    font-size: .8 em;
    color: var(--text-muted);
  }
}

.updated {
  font-size: .9rem;
  color: var(--text-muted);
}
</style>
