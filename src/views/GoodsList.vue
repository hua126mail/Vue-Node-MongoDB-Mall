<template lang="html">
  <div>
    <!-- 头部组件 -->
    <NavHeader></NavHeader>

    <!-- 面包屑组件 -->
    <NavBread>
      <span>Goods</span>
    </NavBread>

    <div class="accessory-result-page accessory-page">
      <div class="container">
        <div class="filter-nav">
          <span class="sortby">Sort by:</span>
          <a href="javascript:void(0)" class="default cur">Default</a>
          <a href="javascript:void(0)" class="price">Price <svg class="icon icon-arrow-short"><use xlink:href="#icon-arrow-short"></use></svg></a>
          <a href="javascript:void(0)" class="filterby stopPop" @click="showFilterPop()">Filter by</a>
        </div>
        <div class="accessory-result">
          <!-- filter -->
          <div class="filter stopPop" id="filter" :class="{'filterby-show':filterByShow}">
            <dl class="filter-price">
              <dt>Price:</dt>
              <dd><a href="javascript:void(0)" :class="{'cur': currentPrice=='all'}">All</a></dd>
              <dd v-for="(item, index) in priceFilter" @click="setPriceFilter(index)">
                <a href="javascript:void(0)" :class="{'cur': currentPrice==index}">{{item.startPrice}} - {{item.endPrice}}</a>
              </dd>
            </dl>
          </div>

          <!-- search result accessories list -->
          <div class="accessory-list-wrap">
            <div class="accessory-list col-4">
              <ul>
                <li v-for="(item, index) in goodsList">
                  <div class="pic">
                    <a href="#"><img v-lazy="'/static/'+item.prodcutImg" alt=""></a>
                  </div>
                  <div class="main">
                    <div class="name">{{item.productName}}</div>
                    <div class="price">{{item.prodcutPrice}}</div>
                    <div class="btn-area">
                      <a href="javascript:;" class="btn btn--m">加入购物车</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="md-overlay" v-show="overLayShow" @click="closePop()"></div>
    <!-- 底部组件 -->
    <NavFooter></NavFooter>
  </div>
</template>

<script>
  import './../assets/css/base.css';
  import './../assets/css/product.css';
  import './../assets/css/login.css';

  import axios from 'axios';
  import NavHeader from '@/components/NavHeader.vue';
  import NavFooter from '@/components/NavFooter.vue';
  import NavBread from '@/components/NavBread.vue';

  export default {
    data(){
      return {
        msg: 'cch',
        goodsList: [],
        currentPrice: 'all',
        priceFilter: [{
          startPrice: 0.00,
          endPrice: 500.00
        },{
          startPrice: 500.00,
          endPrice: 1000.00
        },{
          startPrice: 1000.00,
          endPrice: 2000.00
        }],
        filterByShow: false,
        overLayShow: false
      }
    },
    components: {
      NavHeader,
      NavFooter,
      NavBread
    },
    mounted() {
      this.$nextTick(()=>{
        this.getGoodsList();
      });
    },
    methods: {
      /**
       * [getGoodsList 获取商品信息]
       * @author hua126mail@126.com
       * @return {[type]} [description]
       */
      getGoodsList() {
        axios.get('goods').then((res) => {
          this.goodsList = res.data.result;
        });;
      },
      showFilterPop() {
        this.filterByShow = true;
        this.overLayShow = true;
      },
      setPriceFilter(index) {
        this.currentPrice = index;
        this.chosePop();
      },
      closePop() {
        this.filterByShow = false;
        this.overLayShow = false;
      }
    }
  }
</script>

<style lang="css">
</style>
