<template>
  <div class="cinema_body">
    <Loading v-if="isLoading" />
    <Scroller>
      <ul>
        <!-- <li>
          <div>
            <span>大地电影(澳东世纪店)</span>
            <span class="q"><span class="price">22.9</span> 元起</span>
          </div>
          <div class="address">
            <span>金州区大连经济技术开发区澳东世纪3层</span>  
            <span>1763.5km</span>  
          </div>
          <div class="card">
            <div>小吃</div>
            <div>折扣卡</div>
          </div>
        </li> -->
        <li v-for="item in cinemasList" :key="item.id">
          <div>
            <span>{{item.nm}}</span>
            <span class="q"><span class="price">{{item.sellPrice}}</span> 元起</span>
          </div>
          <div class="address">
            <span>{{item.addr}}</span>  
            <span>{{item.distance}}</span>  
          </div>
          <div class="card">
            <div v-for="(num, key) in item.tag" :key="key" v-if="num===1" :class="key | classCard">{{key | formatCard}}</div>
          </div>
        </li>
      </ul>
    </Scroller>
  </div>
</template>

<script>
export default {
  name: 'CiList',
  data() {
    return {
      cinemasList: [],
      isLoading: true
    }
  },
  mounted() {
    this.axios.get('/api/cinemaList?cityId=10').then((res) => {
      const msg = res.data.msg
      if (msg === 'ok') {
        this.isLoading = false
        this.cinemasList = res.data.data.cinemas
      }
    })
  },
  filters: {
    formatCard(key) {
      const card = [
        {key: 'allowRefund', val: '改签'},
        {key: 'endorse', val: '退'},
        {key: 'sell', val: '折扣卡'},
        {key: 'snack', val: '小吃'}
      ]

      for (let i = 0; i < card.length; i++ ) {
        if (key === card[i].key)
          return card[i].val
      }
      return ''
    },
    classCard(key) {
      const card = [
        {key: 'allowRefund', val: 'bl'},
        {key: 'endorse', val: 'bl'},
        {key: 'sell', val: 'or'},
        {key: 'snack', val: 'or'}
      ]

      for (let i = 0; i < card.length; i++ ) {
        if (key === card[i].key)
          return card[i].val
      }
      return ''
    },
  }
}
</script>

<style scoped>
  #content .cinema_body {
    flex: 1;
    overflow: auto;
  }

  .cinema_body ul {
    padding: 20px;
  }

  .cinema_body li {
    border-bottom: 1px solid #e6e6e6;
    margin-bottom: 20px;
  }

  .cinema_body div {
    margin-bottom: 10px;
  }

  .cinema_body .q {
    font-size: 11px;
    color: #f03d37;
  }

  .cinema_body .price {
    font-size: 18px;
  }

  .cinema_body .address {
    font-size: 13px;
    color: #666;
  }

  .cinema_body .address span:nth-of-type(2) {
    float: right;
  }

  .cinema_body .card {
    display: flex;
  }

  .cinema_body .card div {
    padding: 0 3px;
    height: 15px;
    line-height: 15px;
    border-radius: 2px;
    color: #f90;
    border: 1px solid #f90;
    font-size: 13px;
    margin-right: 5px;
  }

  .cinema_body .card div.or {
    color: #f90;
    border: 1px solid #f90;
  }

  .cinema_body .card div.bl {
    color: #589daf;
    border: 1px solid #589daf;
  }


</style>