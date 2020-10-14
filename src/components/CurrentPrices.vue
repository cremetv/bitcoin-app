<template>
  <swiper
    :slides-per-view="3"
    :space-between="16"
    @swiper="onSwiper"
    @slideChange="onSlideChange">
    <swiper-slide v-for="(value, currency, i) in prices" :key="i">
      <div class="buy">
        <div class="trend-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M12 8.29498L6 14.295L7.41 15.705L12 11.125L16.59 15.705L18 14.295L12 8.29498Z" fill="currentColor"/>
          </svg>
        </div>
        <div class="title">Einkauf</div>
        <div class="price">
          <div class="value">{{ value.buy }}</div>
          <div class="symbol">{{ value.symbol }}</div>
        </div>
      </div>

      <div class="sell">
        <div class="trend-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M12 8.29498L6 14.295L7.41 15.705L12 11.125L16.59 15.705L18 14.295L12 8.29498Z" fill="currentColor"/>
          </svg>
        </div>
        <div class="title">Verkauf</div>
        <div class="price">
          <div class="value">{{ value.sell }}</div>
          <div class="symbol">{{ value.symbol }}</div>
        </div>
      </div>

      <div class="currency">{{ currency }}</div>
    </swiper-slide>
  </swiper>

  <div>
    <a class="refresh" href="#!" @click.prevent="getCurrentPrices">refresh</a>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/swiper-bundle.css'

export default {
  components: {
    Swiper,
    SwiperSlide
  },
  methods: {
    onSwiper (swiper) {
      console.log(swiper)
    },
    onSlideChange () {
      console.log('slide change')
    }
  },
  setup () {
    const prices = ref({})

    const getCurrentPrices = () => {
      fetch('https://blockchain.info/ticker')
        .then(response => response.json())
        .then(data => {
          prices.value = data
        })
    }

    onMounted(() => {
      getCurrentPrices()
    })

    return {
      getCurrentPrices,
      prices
    }
  }
}
</script>

<style scoped lang="less">
@import '../_styles/_mixins.less';
.swiper-container {
  padding: 1rem 0;
}
.swiper-slide {
  background: var(--background-secondary);
  border-radius: 1rem;
  padding: 1rem;
  display: flex;
  flex-direction: row;
  position: relative;

  min-height: 10rem;

  .buy,
  .sell {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: flex-start;

    flex: 1;
  }

  .buy {

  }

  .sell {
    margin-left: 1rem;
  }

  .trend-icon {
    color: var(--text-normal);
    align-self: flex-end;
    margin-bottom: auto;
  }

  .title {
    .fw(--fw-regular);
    color: var(--text-muted);
    font-size: 1.125rem;
    margin-bottom: .5rem;
  }

  .price {
    display: flex;
    flex-direction: row;

    .value {
      font-size: 1.75rem;
      .fw(--fw-semiBold);
      color: var(--text-normal);
    }

    .symbol {
      font-size: 1.75rem;
      .fw(--fw-semiBold);
      color: var(--text-muted);
      margin-left: .5em;
    }
  }

  .currency {
    position: absolute;
    top: 1rem;
    left: 1rem;
    font-size: 1.75rem;
    .fw(--fw-bold);
    color: var(--text-normal);
  }
}
.refresh {
  color: var(--text-muted);
  text-decoration: none;
  transition: color .25s ease;

  .hover-mixin({
    color: var(--focus-primary);
  });
}
</style>
