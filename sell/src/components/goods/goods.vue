<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(item, index) in goods" class="menu-item" :class="{'current':currentIndex === index}"
              @click="selectMenu(index, $event)">
          <span class="text">
           <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span> {{item.name}}
          </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="item in goods" class="food-list food-list-hook">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item">
                <div class="icon">
                  <img :src="food.icon" width="57" height="57">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}</span>
                  </div>
                  <div class="price">
                    <span class="now"> ¥{{food.price}}</span>
                    <span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <Cartcontrol :food="food" @increment="incrementTotal"></Cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <Shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"
                ref="shopcart"></Shopcart>
    </div>
    <food  :food="selectedFood" ref="food"></food>
  </div>
</template>


<script>
  import axios from 'axios'
  import BScroll from 'better-scroll'
  import Shopcart from '../shopcart/shopcart.vue'
  import Cartcontrol from '../cartcontrol/cartcontrol.vue'
  import food from '../food/food.vue'
  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      }
    },
    components: {
      Shopcart: Shopcart,
      Cartcontrol: Cartcontrol,
      food: food
    },
    computed: {
      currentIndex(){
        for (let i = 0; i < this.listHeight.length; i++) {
          let height = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY) >= height && this.scrollY < height2) {
            return i;
          }
        }
        return 0;
      },
      selectFoods(){
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          })
        })
        return foods
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
      axios.get('/api/goods')
        .then(function (response) {
          if (response.data.errno === ERR_OK) {
            var goods = response.data.data
            this.goods = goods;
            this.$nextTick(() => {
              this._initScroll();
              this._calculateHeight();
            })

          }
        }.bind(this))
        .catch(function (error) {
          console.log(error)
        })
    },
    methods: {
      selectFood(food, event){
        if (!event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodScroll = new BScroll(this.$refs.foodsWrapper, {
          probeType: 3,
          click: true
        });
        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calculateHeight(){
        let foodlist = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodlist.length; i++) {
          let item = foodlist[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index, event){
        if (!event._constructed) {
          return;
        }
        let foodlist = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodlist[index];
        this.foodScroll.scrollToElement(el, 300);

      },
      incrementTotal(target) {
        this._drop(target);
      },
      _drop(target){
        this.$nextTick(() => {
          this.$refs.shopcart.drop(target);
        });
      }
    }

  }

</script>


<style>
  .goods {
    display: flex;
    position: absolute;
    width: 100%;
    top: 174px;
    bottom: 46px;
    overflow: hidden;
  }

  .goods .menu-wrapper {
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
  }

  .goods .menu-wrapper .menu-item {
    display: table;
    height: 54px;
    width: 56px;
    line-height: 14px;
    padding: 0 12px;
  }

  .goods .menu-wrapper .menu-item.current {
    background: #fff;
    position: relative;
    margin-top: -1px;
    z-index: 10;
    font-weight: 700;
  }

  .goods .menu-wrapper .menu-item.current .text {
    border: none;
  }

  /*边框*/
  @media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5) {
    .goods .menu-wrapper .menu-item .text:after {
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
    .goods .menu-wrapper .menu-item .text:after {
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

  .goods .menu-wrapper .menu-item .text {
    font-size: 12px;
    display: table-cell;
    width: 56px;
    vertical-align: middle;
    font-size: 12px;
    position: relative;
  }

  .goods .menu-wrapper .menu-item .icon {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    height: 12px;
    margin-right: 2px;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }

  .goods .menu-wrapper .menu-item .icon.decrease {
    background-image: url('decrease_3@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .goods .menu-wrapper .menu-item .icon.decrease {
      background-image: url('decrease_3@3x.png');
      background-repeat: no-repeat;
    }
  }

  .goods .menu-wrapper .menu-item .icon.discount {
    background-image: url('discount_3@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .goods .menu-wrapper .menu-item .icon.discount {
      background-image: url('discount_3@2x.png');
      background-repeat: no-repeat;
    }
  }

  .goods .menu-wrapper .menu-item .icon.guarantee {
    background-image: url('guarantee_3@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .goods .menu-wrapper .menu-item .icon.guarantee {
      background-image: url('guarantee_3@3x.png');
      background-repeat: no-repeat;
    }
  }

  .goods .menu-wrapper .menu-item .icon.invoice {
    background-image: url('invoice_3@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .goods .menu-wrapper .menu-item .icon.invoice {
      background-image: url('invoice_3@3x.png');
      background-repeat: no-repeat;
    }
  }

  .goods .menu-wrapper .menu-item .icon.special {
    background-image: url('special_3@2x.png');
  }

  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .goods .menu-wrapper .menu-item .icon.special {
      background-image: url('special_3@3x.png');
      background-repeat: no-repeat;
    }
  }

  .goods .foods-wrapper {
    flex: 1;
  }

  .goods .foods-wrapper .title {
    padding-left: 14px;
    height: 26px;
    line-height: 26px;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147, 153, 159);
    background: #f3f5f7;
  }

  .goods .foods-wrapper .food-item {
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
    position: relative;
  }

  /*边框*/
  @media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5) {
    .goods .foods-wrapper .food-item:after {
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
    .goods .foods-wrapper .food-item:after {
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

  .goods .foods-wrapper .food-item .icon {
    flex: 0 0 57px;
    margin-right: 10px;
  }

  .goods .foods-wrapper .food-item .content {
    flex: 1;
  }

  .goods .foods-wrapper .food-item .content .name {
    margin: 2px 0 8px 0;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7, 17, 27);
  }

  .goods .foods-wrapper .food-item .content .desc {
    margin-bottom: 8px;
    line-height: 12px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .goods .foods-wrapper .food-item .content .extra {
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .goods .foods-wrapper .food-item .content .count {
    margin-right: 12px;
  }

  .goods .foods-wrapper .food-item .content .price {
    font-weight: 700;
    line-height: 24px;
  }

  .goods .foods-wrapper .food-item .content .price .now {
    margin-right: 8px;
    font-size: 14px;
    color: rgb(240, 20, 20);
  }

  .goods .foods-wrapper .food-item .content .price .old {
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .goods .foods-wrapper .food-item .content .cartcontrol-wrapper {
    position: absolute;
    right: 0;
    bottom: 12px;
  }

  .goods .foods-wrapper .food-item:last-child {
    border: none;
    margin-bottom: 0;
  }
</style>
