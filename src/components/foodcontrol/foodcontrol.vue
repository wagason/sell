<template>
  <div class="foodcontrol">
    <transition name="move">
      <span v-show="food.count" class="sub" @click="subFood">
        <span class="inner icon-remove_circle_outline"></span>
      </span>
    </transition>
    <span class="count">{{food.count}}</span>
    <span class="add icon-add_circle" @click="addFood"></span>
  </div>
</template>

<script>
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object,
        default: () => {
          return {}
        }
      }
    },
    methods: {
      addFood () {
        if (!event._constructed) return
        if (this.food.count === undefined) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
      },
      subFood () {
        if (!event._constructed) return
        if (this.food.count !== undefined && this.food.count > 0) {
          this.food.count--
        }
      }
    }
  }
</script>


<style lang="stylus" ref="stylesheet/stylus">
.foodcontrol
  font-size: 0
  .sub
    display inline-block
    vertical-align top
    padding 6px
    transition all 0.4s linear
    &.move-enter-to
      opacity 1
      transform translate3d(0, 0, 0)
      .inner
        transform rotate(0deg)       
    &.move-enter, &.move-leave-to
      opacity 0
      transform translate3d(24px, 0, 0)
      .inner
        transform rotate(180deg)
    .inner
      display inline-block
      line-height 24px
      font-size: 24px
      color rgb(0, 160, 220)
      transition all 0.4s linear
  .count
    vertical-align top
    margin-top: 6px
    display inline-block
    text-align: center
    width: 12px
    line-height 24px
    font-size: 10px
    color rgb(147, 157, 159)
  .add
    display inline-block
    vertical-align top
    padding 6px
    line-height 24px
    font-size: 24px
    color rgb(0, 160, 220)

</style>
