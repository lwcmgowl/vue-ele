<template>
  <div>
    <div class="header">
      <div class="content-wraper">
        <div class="avatar"><img :src="seller.avatar" alt="" width="64" height="64"></div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="support">
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div class="support-count" v-if="seller.supports" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="notice-wraper" @click="showDetail">
        <span class="notice-title"></span><span class="notice-text">{{seller.bulletin}}</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
      <div class="background">
        <img :src="seller.avatar" alt="" width="100%" height="100%">
      </div>
      <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <Star :size="48" :score="seller.score"></Star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
               <li class="support-item" v-for="(item,index) in seller.supports">
                 <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                 <span class="text">{{seller.supports[index].description}}</span>
               </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
      </transition>
    </div>
  </div>

</template>


<script>
  import Star from '../star/star'
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        detailShow:false
      }
    },
    methods:{
      showDetail(){
          this.detailShow=true
      },
      hideDetail(){
        this.detailShow=false
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];

    },
    components:{
      Star:Star
    }
  }

</script>


<style>
  .header {
    color: #fff;
    /*background: #999;*/
    position: relative;
    background: rgba(7,17,27,.5);
    overflow: hidden;
  }

  .header .content-wraper {
    padding: 24px 12px 18px 24px;
    font-size: 0;
    position: relative;
  }

  .header .content-wraper .avatar {
    display: inline-block;
    vertical-align: top;
  }

  .header .content-wraper .avatar img {
    border-radius: 2px;
  }

  .header .content-wraper .content {
    display: inline-block;
    /*font-size: 14px;*/
    margin-left: 14px;
  }

  .header .content-wraper .content .title {
    margin: 2px 0 8px 0;
  }

  .header .content-wraper .content .title .brand {
    width: 30px;
    height: 18px;
    display: inline-block;
    vertical-align: top;
    background-image: url('brand@2x.png');
    background-size: 30px 18px;
    background-repeat: no-repeat;
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .content-wraper .content .title .brand {
      background-image: url('brand@3x.png');
      background-repeat: no-repeat;
    }
  }

  .header .content-wraper .content .title .name {
    font-weight: bold;
    margin-left: 6px;
    font-size: 16px;
    line-height: 18px;
  }

  .header .content-wraper .content .description {
    margin-bottom: 10px;
    line-height: 12px;
    font-size: 12px;
  }

  .header .content-wraper .content .support .icon {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }

  .header .content-wraper .content .support .icon.decrease {
    background-image: url('decrease_1@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .content-wraper .content .support .icon.decrease {
      background-image: url('decrease_1@2x.png');
      background-repeat: no-repeat;
    }
  }

  .header .content-wraper .content .support .icon.discount {
    background-image: url('discount_1@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .content-wraper .content .support .icon.discount {
      background-image: url('discount_3@2x.png');
      background-repeat: no-repeat;
    }
  }

  .header .content-wraper .content .support .icon.guarantee {
    background-image: url('guarantee_1@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .content-wraper .content .support .icon.guarantee {
      background-image: url('guarantee_2@2x.png');
      background-repeat: no-repeat;
    }
  }

  .header .content-wraper .content .support .icon.invoice {
    background-image: url('invoice_1@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .content-wraper .content .support .icon.invoice {
      background-image: url('invoice_2@2x.png');
      background-repeat: no-repeat;
    }
  }

  .header .content-wraper .content .support .icon.special {
    background-image: url('special_1@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .content-wraper .content .support .icon.special {
      background-image: url('special_3@2x.png');
      background-repeat: no-repeat;
    }
  }

  .header .content-wraper .content .support .text {
    font-size: 10px;
    line-height: 12px;
  }
  .header .content-wraper .support-count{
    position:absolute;
    right: 12px;
    bottom:14px;
    padding:0 8px;
    height: 24px;
    line-height: 24px;
    border-radius: 14px;
    background:rgba(0,0,0,.2);
    text-align: center;
  }
  .header .content-wraper .support-count .count{
    font-size: 10px;
    vertical-align: top;
  }
  .header .content-wraper .support-count .icon-keyboard_arrow_right{
    font-size: 10px;
    line-height: 24px;
    margin-left: 2px;
  }
  .header .content-wraper .support-count .icon-keyboard_arrow_right{
    font-size: 10px;
  }
  .header .notice-wraper{
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background: rgba(7,17,27,.2);
    position: relative;
  }
  .header .notice-wraper .notice-title{
    vertical-align: top;
    display: inline-block;
    margin-top: 8px;
    width: 22px;
    height: 12px;
    background-image: url('bulletin@2x.png');
    background-size: 22px 12px;
    background-repeat: no-repeat;
  }
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .notice-wraper .notice-title{
      background-image: url('bulletin@3x.png');
    }
  }
  .header .notice-wraper .notice-text{
    vertical-align: top;
    font-size: 10px;
    margin:0 4px;
  }
  .header .notice-wraper .icon-keyboard_arrow_right{
   position: absolute;
    font-size: 10px;
    right: 12px;
    top:10px;
  }
  .header .background{
    position: absolute;
    top:0;
    left:0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }
  .header .detail{
    top:0;
    left: 0;
    position:fixed;
    z-index: 100;
    width:100%;
    height: 100%;
    overflow: auto;
    transition: all 0.5s;
    background:rgba(7,17,27,.8);
    backdrop-filter:blur(10px);
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-active {
    opacity: 0
  }
  .header .detail .detail-wrapper{
    min-height:100%;
    width: 100%;
  }
  .header .detail .detail-wrapper .detail-main{
    margin-top: 64px;
    padding-bottom:64px;
  }
  .header .detail .detail-wrapper .detail-main .name{
    line-height: 16px;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
  }
  .header .detail .detail-wrapper .detail-main .star-wrapper{
    margin-top: 18px;
    padding:2px 0;
    text-align: center;
  }
  .header .detail .detail-wrapper .title{
    display: flex;
    width:80%;
    margin:30px auto;
  }
  .header .detail .detail-wrapper .title .line{
    flex: 1;
    position: relative;
    top:-6px;
    border-bottom: 1px solid rgba(255,255,255,.2);
  }
  .header .detail .detail-wrapper .title .text{
    padding:0 12px;
    font-size:14px ;
    font-weight: bold;
  }
  .header .detail .detail-wrapper .supports{
    width: 80%;
    margin: 0 auto;
  }
  .header .detail .detail-wrapper .supports .support-item{
    padding:0 12px;
    margin-bottom: 12px;
    font-size: 0;
  }
  .header .detail .detail-wrapper .supports .support-item:last-child{
    margin-bottom: 0;
  }
  .header .detail .detail-wrapper .supports .support-item .icon{
    display: inline-block;
    width:16px;
    height: 16px;
    vertical-align: top;
    margin-right: 6px;
    background-size: 16px 16px;
    background-repeat: no-repeat;
  }
  .header .detail .detail-wrapper .supports .support-item .text{
    line-height: 16px;
    font-size: 12px;
  }
  .header .detail .detail-wrapper .supports .support-item .icon.decrease {
    background-image: url('decrease_2@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .detail .detail-wrapper .supports .support-item .icon.decrease {
      background-image: url('decrease_2@3x.png');
      background-repeat: no-repeat;
    }
  }

  .header .detail .detail-wrapper .supports .support-item .icon.discount {
    background-image: url('discount_2@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .detail .detail-wrapper .supports .support-item .icon.discount {
      background-image: url('discount_2@3x.png');
      background-repeat: no-repeat;
    }
  }

  .header .detail .detail-wrapper .supports .support-item .icon.guarantee {
    background-image: url('guarantee_2@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .detail .detail-wrapper .supports .support-item .icon.guarantee {
      background-image: url('guarantee_2@3x.png');
      background-repeat: no-repeat;
    }
  }

  .header .detail .detail-wrapper .supports .support-item .icon.invoice {
    background-image: url('invoice_2@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .detail .detail-wrapper .supports .support-item .icon.invoice {
      background-image: url('invoice_2@3x.png');
      background-repeat: no-repeat;
    }
  }

  .header .detail .detail-wrapper .supports .support-item .icon.special {
    background-image: url('special_2@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .header .detail .detail-wrapper .supports .support-item .icon.special {
      background-image: url('special_2@3x.png');
      background-repeat: no-repeat;
    }
  }
  .header .detail .detail-wrapper .bulletin{
    width: 80%;
    margin: 0 auto;
  }
  .header .detail .detail-wrapper .bulletin .content{
    padding:0 12px;
    line-height: 24px;
    font-size: 12px;
  }
  .header .detail  .detail-close{
    position:relative ;
    width: 32px;
    height: 32px;
    margin:-64px auto 0 auto;
    clear: both;
    font-size: 32px;
  }
</style>
