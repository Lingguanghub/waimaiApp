<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
     </transition>
    <!--==================================-->
    <div class="cart-count" v-show="food.count>0">{{ food.count }}</div>
    <!--=========================================-->
    <div class="cart-add icon-add_circle" @click="addCart">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart (event) {
      if (!this.food.count) {
        this.$set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      // 触发抛小球事件,通过￥emit发射一个add，在goods组件里接受并实现方法
      this.$emit('add', event.target)
    },
    decreaseCart () {
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
.cartcontrol
  font-size: 0
  .cart-decrease
    display: inline-block
    padding: 6px
    opacity: 1
    transform: translate3d(0, 0, 0)
    .inner
      display: inline-block
      line-height: 24px
      font-size: 24px
      color: rgb(0,160,220)
      vertical-align: middle
      transform: rotate(0deg)
      transition: all 0.4s linear
    &.move-enter-active,&.move-leave-active
      transition: all 0.4s linear
    &.move-enter,&.move-leave-active
      opacity: 0
      transform: translate3d(24px, 0, 0)
      .inner
        transform: rotate(180deg)
  .cart-count
    display: inline-block
    width: 16px
    line-height: 24px
    height: 24px
    text-align: center
    font-size: 10px
    color: rgb(147,153,159)
    vertical-align: middle
  .cart-add
    display: inline-block
    padding: 6px
    line-height: 24px
    font-size: 24px
    color: rgb(0,160,220)
    vertical-align: middle

</style>
