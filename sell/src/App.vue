<template>
  <div id="app">
    <v-Header :seller="seller"></v-Header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <div class="content">
      <router-view :seller="seller"></router-view>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {urlParse} from './common/js/util'
  import header from './components/header/header.vue'
  import axios from 'axios'
  const ERR_OK = 0
  export default {
    data() {
      return {
        seller: {
          id:(()=>{
            let queryParam=urlParse();
            return queryParam.id;
          })()
        }
      };
    },
    created () {
      var self = this;
      axios.get('/api/seller?id='+this.seller.id)
        .then(function (response) {
          if (response.data.errno === ERR_OK) {
            var sellers = response.data.data
            //self.seller = sellers;
            self.seller = Object.assign({}, self.seller, sellers);
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    components: {'v-Header': header}
  }
</script>

<style>
  #app .tab {
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    position: relative;
  }

  @media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5) {
    #app .tab:after {
      display: block;
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      border-top: 1px solid rgba(7, 17, 27, .1);
      content: " ";
      transform: scaleY(0.7);
      -webkit-transform: scaleY(0.7);
    }
  }

  @media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2) {
    #app .tab:after {
      display: block;
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      border-top: 1px solid rgba(7, 17, 27, .1);
      content: " ";
      transform: scaleY(0.5);
      -webkit-transform: scaleY(0.5);
    }
  }

  /*#app .tab:after{*/
  /*display: block;*/
  /*position: absolute;*/
  /*left: 0;*/
  /*bottom:0;*/
  /*width:100%;*/
  /*border-top: 1px solid rgba(7,17,27,.1) ;*/
  /*content:" ";*/
  /*}*/
  #app .tab .tab-item {
    flex: 1;
    text-align: center;
  }

  #app .tab .tab-item a {
    display: block;
    font-size: 14px;
    color: rgb(77, 85, 93);
  }

  #app .tab .tab-item a.active {
    color: rgb(240, 20, 20);
  }
</style>
