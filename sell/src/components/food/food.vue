<template>
  <transition name="move">
    <div class="food" v-show="showFlag" ref="food">
      <div class="food-content">
        <div class="img-header">
          <img :src="food.image" alt="">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <div><h1 class="title">{{food.name}}</h1></div>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now"> ¥{{food.price}}</span>
            <span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <Cartcontrol :food="food" @increment="incrementTotal"></Cartcontrol>
          </div>
          <transition name="fade">
            <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count===0">
              加入购物车
            </div>
          </transition>
        </div>
        <Split v-show="food.info"></Split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <div class="text">{{food.info}}</div>
        </div>
        <Split></Split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <Ratingselect  @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"
                        :ratings="food.ratings"></Ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" :src="rating.avatar" alt="" width="12" height="12">
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <p class="text">
                  <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                  {{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
              暂无评价
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>


<script>
  import BScroll from 'better-scroll'
  import Vue from 'vue'
  import {formatDate} from '../../common/js/date';
  import Cartcontrol from '../cartcontrol/cartcontrol.vue'
  import Split from '../split/split.vue'
  import Ratingselect from '../ratingselect/ratingselect.vue'

//  const POSITIVE = 0;
//  const NEGATIVE = 1;
  const ALL = 2;

  export default{
    props: {
      food: {
        type: Object
      }
    },
    data(){
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      }
    },
    methods: {
      show(){
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = false;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            })
          } else {
            this.scroll.refresh();
          }
        })
      },
      toggleContent() {
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      hide(){
        this.showFlag = false;
      },
      addFirst(event){
        if (!event._constructed) {
          return;
        }
        Vue.set(this.food, 'count', 1)
        this.$emit('increment', event.target);
      },
      incrementTotal(target) {
        this.$emit('increment', target);
      },
      selectRating(type) {
        this.selectType = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      needShow(type, text){
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type = this.selectType;
        }
      }
    },
    filters:{
      formatDate(time){
          let date=new Date(time);
          return formatDate(date,'yyyy-MM-dd hh:mm');
      }
    },
    components: {
      Cartcontrol: Cartcontrol,
      Split: Split,
      Ratingselect: Ratingselect
    }
  }

</script>


<style>
  .food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #fff;
    transform: translate3d(0, 0, 0)
  }

  .food.move-enter-active, .food.move-leave-active {
    transition: all 0.2s linear;
  }

  .food.move-enter, .food.move-leave-active {
    transform: translate3d(100%, 0, 0);
  }

  .food .img-header {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
  }

  .food .img-header img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .food .img-header .back {
    position: absolute;
    top: 10px;
    left: 0;
  }

  .food .img-header .back .icon-arrow_lift {
    display: block;
    padding: 10px;
    font-size: 20px;
    color: #fff;
  }

  .food .content {
    padding: 18px;
    position: relative;
  }

  .food .content .title {
    line-height: 14px;
    margin-bottom: 8px;
    font-size: 14px;
    font-weight: 700;
    color: rgb(7, 17, 27);
  }

  .food .content .detail {
    margin-bottom: 18px;
    line-height: 10px;
    font-size: 0;
    height: 10px;
  }

  .food .content .detail .sell-count {
    font-size: 10px;
    color: rgb(147, 153, 159);
    margin-right: 12px;
  }

  .food .content .detail .rating {
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .food .content .price {
    font-weight: 700;
    line-height: 24px;
  }

  .food .content .price .now {
    margin-right: 8px;
    font-size: 14px;
    color: rgb(240, 20, 20);
  }

  .food .content .price .old {
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .food .cartcontrol-wrapper {
    position: absolute;
    right: 12px;
    bottom: 12px;
  }

  .food .buy {
    position: absolute;
    right: 18px;
    bottom: 18px;
    z-index: 10;
    height: 24px;
    line-height: 24px;
    padding: 0 12px;
    box-sizing: border-box;;
    font-size: 10px;
    border-radius: 12px;
    color: #fff;
    background: rgb(0, 160, 220);
    opacity: 1;
  }

  .food .buy.fade-enter-active, .food .buy.fade-leave-active {
    transition: all 0.2s;
  }

  .food .buy.fade-enter, .food .buy.fade-leave-active {
    opacity: 0;
    z-index: -1;
  }

  .food .info {
    padding: 18px;
  }

  .food .info .title {
    line-height: 14px;
    margin-bottom: 6px;
    font-size: 14px;
    color: rgb(7, 17, 27);
    font-weight: 700;
  }

  .food .info .text {
    line-height: 24px;
    padding: 0 8px;
    font-size: 12px;
    color: rgb(77, 85, 93);
  }

  .food .rating {
    padding-top: 18px;
  }

  .food .rating .title {
    line-height: 14px;
    margin-left: 18px;
    font-size: 14px;
    color: rgb(7, 17, 27);
    font-weight: 700;
  }

  .food .rating .rating-wrapper {
    padding: 0 18px;
  }

  .food .rating .rating-wrapper .rating-item {
    position: relative;
    padding: 16px 0;
    border-bottom: 1px solid rgba(7, 17, 27, .1);
  }

  .food .rating .rating-wrapper .rating-item .user {
    position: absolute;
    right: 0;
    top: 16px;
    font-size: 0;
    line-height: 12px;
  }

  .food .rating .rating-wrapper .rating-item .user .name {
    display: inline-block;
    vertical-align: top;
    font-size: 10px;
    color: rgb(147, 153, 159);
    margin-right: 6px;
  }

  .food .rating .rating-wrapper .rating-item .user .avatar {
    border-radius: 50%;
  }

  .food .rating .rating-wrapper .rating-item .time {
    margin-bottom: 6px;
    line-height: 12px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .food .rating .rating-wrapper .rating-item .text {
    line-height: 16px;
    font-size: 12px;
    color: rgb(7, 17, 27);
  }

  .food .rating .rating-wrapper .rating-item .text .icon-thumb_up, .food .rating .rating-wrapper .rating-item .text .icon-thumb_down {
    line-height: 16px;
    margin-right: 4px;
    font-size: 12px;
  }

  .food .rating .rating-wrapper .rating-item .text .icon-thumb_up {
    color: rgb(0, 160, 220);
  }

  .food .rating .rating-wrapper .rating-item .text .icon-thumb_down {
    color: rgb(147, 153, 159);
  }
  .food .rating .rating-wrapper .no-rating{
    padding: 16px 0;
    font-size: 12px;
    color: rgb(147, 153, 159);
  }


</style>
