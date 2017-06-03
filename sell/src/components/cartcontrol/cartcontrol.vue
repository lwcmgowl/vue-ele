<template>
  <div class="cartcontrol">
    <transition name="fade">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <transition name="inner ">
          <span class="inner icon-remove_circle_outline"></span>
        </transition>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>

</template>


<script>
  import Vue from 'vue'
  export default{
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event){
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
//              this.food.count=1;
        } else {
          this.food.count++;
        }
        this.$emit('increment', event.target)
      },
      decreaseCart(event){
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  }

</script>


<style>
  .cartcontrol {
    font-size: 0;
  }

  .cartcontrol .cart-decrease {
    display: inline-block;
    padding: 6px;
  }
  .cartcontrol .cart-decrease.fade-enter-active,.cartcontrol .cart-decrease.fade-leave-active{
    transition: all 0.4s linear
  }
  .cartcontrol .cart-decrease.fade-enter,.cartcontrol .cart-decrease.fade-leave-active{
    opacity: 0;
    transform:translate3d(24px, 0, 0);
  }
  .cartcontrol .cart-decrease .inner {
    line-height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
    display: inline-block;
  }
  .cartcontrol .cart-decrease .inner.inner-enter-active,.cartcontrol .cart-decrease .inner.inner-leave-active{
    transition: all 0.4s linear;
    transform: rotate(0);
  }
  .cartcontrol .cart-decrease .inner.inner-enter,.cartcontrol .cart-decrease .inner.inner-leave-active{
    opacity: 0;
    transform:rotate(180deg);
  }

  .cartcontrol .cart-count {
    display: inline-block;
    font-size: 10px;
    vertical-align: top;
    width: 12px;
    padding-top: 6px;
    line-height: 24px;
    text-align: center;
    color: rgb(147, 153, 159);
  }

  .cartcontrol .cart-add {
    display: inline-block;
    line-height: 24px;
    font-size: 24px;
    padding: 6px;
    color: rgb(0, 160, 220)
  }
</style>
