<template>
  <section style="z-index:2100">
    <!--<mt-search v-model="searchData" cancel-text="取消" placeholder="中文/拼音/三字码">-->
      <!--<ul style="background: #fff;">-->
        <!--<li class="city-cell" v-for="item in searchCityData" @click="changecity(item.Name)">{{item.Name}}-->
          <!--<span>{{item.CityCode}}</span>-->
        <!--</li>-->
      <!--</ul>-->
    <!--</mt-search>-->
    <!--<button class="mint-button mint-button&#45;&#45;default mint-button&#45;&#45;normal search-back" @click="offPopup">-->
    <!--<span class="mint-button-icon">-->
    <!--<i class="mintui mintui-back"></i>-->
    <!--</span>-->
    <!--</button>-->
    <mt-index-list>
      <li class="mint-indexsection"><p class="mint-indexsection-index">热门城市</p>
        <ul class="hot-city">
          <li class="hot-city-cell" v-for="item in Hcity" :key="item.Code" @click="changecity(item.Name)">{{item.Name}}</li>
        </ul>
      </li>
      <mt-index-section v-for="(item,key) in resolveCityData" :key="key" :index="key" style="clear: both">
        <li class="city-cell" v-for="it in item" :key="it.Code" @click="changecity(it.Name)">{{it.Name}}
          <span>{{it.CityCode}}</span>
        </li>
      </mt-index-section>
    </mt-index-list>
  </section>
</template>

<script>
  import {mapGetters} from 'vuex';
  import { Hcity, Cdata } from '@/assets/js/city.data'

  export default {
    props: {
      Tcity: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        searchData: '',
        searchCityData: [],
        Cdata,
        Hcity
      }
    },
    computed: {
//      ...mapState('base', ['targetCity', 'startCity', 'endCity', 'currentCity']),
      ...mapGetters({
        currentCity: 'currentCity'
      }),
      resolveCityData() {
        let ob = {}
        this.Cdata.Cities.forEach((cities) => {
          let key = cities.Code[0];
          if (ob[key] === undefined) {
            ob[key] = [];
          }
          ob[key].push(cities);
        });
        return ob;
      }
    },
    methods: {
//      ...mapMutations('base', ['changeStaticState']),
      changecity(val) {
        this.searchData = '';
//            $('.mint-indexlist-content').eq(0).scrollTop(0);
        this.$store.dispatch('changeCityState', {
          name: 'showCity',
          newVal: false
        });
        this.$store.dispatch('changeCityState', {
          name: this.Tcity,
          newVal: val
        });
      },
      offPopup() {
        this.$store.dispatch('changeCityState', {
          name: 'showCity',
          newVal: false
        });
      }

    },
    watch: {
      searchData(newValue) {
//            if (newValue === '') {
//                $('.mint-searchbar-cancel').hide()
//                // $('.mint-search').height(44);
//                this.searchCityData = [];
//            } else {
//                $('.mint-searchbar-cancel').show();
//                // $('.mint-search').height('100%');
//                this.searchCityData = this.Cdata.Cities.filter(cities => cities.CityCode.toLowerCase().indexOf(this.searchData.toLowerCase()) !== -1 || cities.Code.toLowerCase().indexOf(this.searchData.toLowerCase()) !== -1 || cities.Name.indexOf(this.searchData) !== -1);
//            }
      }
    }
  }
</script>
<style scoped type="text/less" lang="less">
  section {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #fff;
    ul {
      list-style: none;
      .mint-cell {
        border-bottom: 1px solid #ddd;
      }
      .city-cell {
        line-height: 2.5rem /* 116/108 */;
        border-bottom: 1px solid #ddd;
        padding: 0 10px;
        font-size: 16px;
        position: relative;
        span {
          float: left;
          position: absolute;
          left: 20%;
          color: #7f7f7f;
          font-size: 14px;
        }
      }
    }
  }

  .hot-city {
    padding: .2222rem /* 24/108 */ .1852rem /* 20/108 */;
    padding-top: 0;
    background: #f5f5f5;
  }

  .hot-city-cell {
    float: left;
    width: 30%;
    line-height: 2rem /* 90/108 */;
    text-align: center;
    margin-right: .1852rem /* 20/108 */;
    margin-bottom: .2222rem /* 24/108 */;
    border: 1px solid #e3e3e3;
    background: #fff;
    font-size: 16px;
    &.active {
      border-color: #fcca7e;
    }
    &.current-place i {
      color: #fcca7e;
      margin-left: -8px;
    }
  }

  .search-back {
    position: absolute;
    top: 0;
    left: 10px;
    background-color: transparent;
    border: 0;
    box-shadow: none;
    color: inherit;
    display: inline-block;
    padding: 0;
    font-size: inherit;
    z-index: 101;
    i {
      font-size: 20px;
      color: #fff;
    }
  }
</style>
