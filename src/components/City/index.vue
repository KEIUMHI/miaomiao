<template>
  <div class="city_body">
    <div class="city_list">
      <Loading v-if="isLoading" />
      <Scroller ref="city_List">
        <div>
          <div class="city_hot">
            <h2>热门城市</h2>
            <ul class="clearfix">
              <li v-for="item in hotList" :key="item.id" @tap="handleToCity(item.nm, item.id)">{{item.nm}}</li>
            </ul>
          </div>
          <div class="city_sort" ref="city_sort">
            <div v-for="item in cityList" :key="item.index">
              <h2>{{item.index}}</h2>
              <ul>
                <li v-for="itemList in item.list" :key="itemList.id" @tap="handleToCity(itemList.nm, itemList.id)">{{itemList.nm}}</li>
              </ul>
            </div>
          </div>
        </div>
      </Scroller>
    </div>
    <div class="city_index">
      <ul>
        <li v-for="(item,index) in cityList" :key="item.index" @touchstart="handleToIndex(index)">{{item.index}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'City',
  data(){
    return {
      cityList: [],
      hotList: [],
      isLoading: true
    }
  },
  mounted() {
    const cityList = window.localStorage.getItem('cityList')
    const hotList = window.localStorage.getItem('hotList')

    if (cityList && hotList) {
      this.isLoading = false
      this.cityList = JSON.parse(cityList)
      this.hotList = JSON.parse(hotList)
      
    } else {
      this.axios.get('/api/cityList').then((res) => {
        const msg = res.data.msg;
        if (msg === 'ok'){
          this.isLoading = false
          const cities = res.data.data.cities;
          var {cityList, hotList} = this.formatCityList(cities);
          this.cityList = cityList;
          this.hotList = hotList;
          window.localStorage.setItem('cityList', JSON.stringify(cityList))
          window.localStorage.setItem('hotList', JSON.stringify(hotList))
        }
      })
    }
  },
  methods: {

    formatCityList(cities) {
      let cityList = [];
      let hotList = [];

      for (let i = 0; i < cities.length; i++){
        if (cities[i].isHot === 1)
          hotList.push(cities[i]);
      }

      for (var i = 0; i < cities.length; i++){
        var fristLetter = cities[i].py.substring(0,1).toUpperCase();
        if (toCom(fristLetter)) {
          createCityList();
        } else {
          addCityList();
        }
      }

      cityList.sort((a,b) => {
        if (a.index > b.index) 
          return 1;
        else if (a.index < b.index)
          return -1;
        else 
          return 0;
      })

      function toCom(fristLetter) {
        for (let i = 0; i < cityList.length; i++){
          if (cityList[i].index === fristLetter)
            return false;
        }
        return true;
      }

      // 向cityList结果集中增建index索引并push对应城市信息至list属性中
      function createCityList() {
        cityList.push( { index: fristLetter, list: [{ nm: cities[i].nm, id: cities[i].id }] } );
      }

      // 向cityList结果集List属性中push城市信息
      function addCityList() {
        for (let j = 0; j < cityList.length; j++) {
          if (cityList[j].index === fristLetter)
            cityList[j].list.push( { nm: cities[i].nm, id: cities[i].id } );
        }
      }

      return {
        cityList,
        hotList
      }
    },

    handleToIndex(index) {
      var h2 = this.$refs.city_sort.getElementsByTagName('h2')
      // this.$refs.city_sort.parentNode.scrollTop = h2[index].offsetTop;
      this.$refs.city_List.toScrollTop(-h2[index].offsetTop)
    },

    handleToCity(nm, id) {
      this.$store.commit('city/CITY_INFO', {nm, id})
      window.localStorage.setItem('nowNm', nm)
      window.localStorage.setItem('nowId', id)
      this.$router.push('movie/nowPlaying')
    }
  }
}
</script>
 
<style scoped>
  #content .city_body {
    margin-top: 45px;
    display: flex;
    width: 100%;
    position: absolute;
    top: 0;
    bottom: 0;
  }

  .city_body .city_list {
    flex: 1;
    overflow: auto;
    background: #FFF5F0;
  }

  .city_body .city_list::-webkit-scrollbar {
    background-color: transparent;
    width: 0;
  }

  .city_body .city_hot {
    margin-top: 20px;
  }

  .city_body .city_hot h2 {
    padding-left: 15px;
    line-height: 30px;
    font-size: 14px;
    background: #F0F0F0;
    font-weight: normal;
  }

  /* .city_body .city_hot ul {} */

  .city_body .city_hot ul li {
    float: left;
    background: #fff;
    width: 29%;
    height: 33px;
    margin-top: 15px;
    margin-left: 3%;
    padding: 0 4px;
    border: 1px solid #e6e6e6;
    border-radius: 3px;
    line-height: 33px;
    text-align: center;
    box-sizing: border-box;
  }

  /* .city_body .city_sort {} */

  .city_body .city_sort div {
    margin-top: 20px;
  }

  .city_body .city_sort h2 {
    padding-left: 15px;
    line-height: 30px;
    font-size: 14px;
    background: #F0F0F0;
    font-weight: normal;
  }

  .city_body .city_sort ul {
    padding-left: 10px;
    margin-top: 10px;
  }

  .city_body .city_sort ul li {
    line-height: 30px;
  }

  .city_body .city_index {
    width: 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    border-left: 1px solid #e6e6e6;
  }

</style>
