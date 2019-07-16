<template>
    <div class="shopcart-wrap">
      <div class="content">
        <div class="content-left">
          <div class="icon-box" :class="{ 'highlight': totalCount > 0}">
            <div class="icon">
              <span class="icon-shopping_cart"></span>
            </div>
            <span v-show="totalCount > 0" class="count">{{totalCount}}</span>
          </div>
          <div class="price" :class="{ 'highlight': totalCount > 0}">¥{{ totalPrice }}</div>
          <div class="desc">另需配送费¥{{deliveryPrice}}元</div>
        </div>
        <div class="content-right">
          <div class="pay" :class="payClass">
            {{payDesc}}
          </div>
        </div>
      </div>
      <div class="ball-wrap">
        <div v-for="ball in balls">
          <transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
            <div class="ball" v-show="ball.show">
              <div class="inner inner-hook"></div>
            </div>
          </transition>
        </div>
      </div>
    </div>
</template>

<script>
  import bus from '@/common/bus.js'
  export default {
    data () {
      return {
        balls: [
          {
            id: 1,
            show: false
          },
          {
            id: 2,
            show: false
          },
          {
            id: 3,
            show: false
          },
          {
            id: 4,
            show: false
          },
          {
            id: 5,
            show: false
          }
        ],
        dropBalls: []
      }
    },
    props: {
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
      selectedFoods: {
        type: Array,
        default: () => {
          return []
        }
      }
    },
    computed: {
      totalPrice () {
        let totalPrice = 0
        this.selectedFoods.forEach((food) => {
          totalPrice += food.price * food.count
        })
        return totalPrice
      },
      totalCount () {
        let totalCount = 0
        this.selectedFoods.forEach((food) => {
          totalCount += food.count
        })
        return totalCount
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return '¥' + this.minPrice + '元起送'
        } else if (this.minPrice > this.totalPrice) {
          let diff = this.minPrice - this.totalPrice
          return `还差¥${diff}元起送`
        } else if (this.minPrice <= this.totalPrice) {
          return '去结算'
        }
      },
      payClass () {
        if (this.minPrice > this.totalPrice) {
          return 'not-enough'
        } else {
          return 'enough'
        }
      }
    },
    created () {
      var self = this
      bus.$on('addFood', function (el) {
        self.drop(el)
      })
    },
    methods: {
      beforeDrop (el) {
        let ball = this.dropBalls[this.dropBalls.length - 1]

        let rect = ball.el.getBoundingClientRect()
        let x = rect.left - 32
        let y = -(window.innerHeight - rect.top - 22)
        el.style.display = ''
        el.style.webkitTransform = `translate3d(0,${y}px,0)`
        el.style.transform = `translate3d(0,${y}px,0)`
        let inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = `translate3d(${x}px,0,0)`
        inner.style.transform = `translate3d(${x}px,0,0)`
      },
      dropping (el, done) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0,0,0)'
          el.style.transform = 'translate3d(0,0,0)'
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = 'translate3d(0,0,0)'
          inner.style.transform = 'translate3d(0,0,0)'
          el.addEventListener('transitionend', done)
        })
      },
      afterDrop (el) {
        let ball = this.dropBalls.shift()
        if (ball) {
          ball.show = false
          el.style.display = 'none'
        }
      },
      drop (el) {
        for (var i = 0; i < this.balls.length; i++) {
          var ball = this.balls[i]
          if (!ball.show) {
            ball.el = el
            ball.show = true
            this.dropBalls.push(ball)
            return
          }
        }
      }
    }
  }
</script>

<style lang="stylus" ref="stylesheet/stylus">
.shopcart-wrap
  position: fixed
  left: 0
  bottom: 0
  z-index: 50
  width: 100%
  height: 48px
  .content
    display: flex
    background-color: #141d27
    font-size 0
    color rgb(255,255,255,0.4)
    .content-left
      flex 1
      .icon-box
        display inline-block
        position relative
        top: -10px
        box-sizing: border-box
        margin 0 12px
        padding 6px
        width 56px
        height 56px
        background-color: #141d27
        vertical-align: top
        border-radius: 50%
        background-color: #141d27
        &.highlight 
          .icon
            background-color #00a0dc
            .icon-shopping_cart
              color #ffffff
        .count
          position absolute
          top 0
          right 0
          width 24px 
          height 16px
          text-align center
          line-height 16px
          font-size 10px
          color #fff
          
          border-radius 16px
          background-color red
        .icon
          width 100%
          height 100%
          background-color #2b343c
          text-align center
          border-radius 50%
          .icon-shopping_cart
            line-height 44px
            font-size 24px
            color: #80858a
      .price
        display inline-block
        vertical-align top
        margin-top 12px
        padding-right 12px
        border-right 1px solid rgba(255,255,255, 0.1)
        line-height 24px
        font-size 16px
        font-weight 700
        color rgba(255,255,255, 0.4)
        &.highlight
          color: #fff
      .desc
        display inline-block
        vertical-align top
        margin 12px 0 0 12px
        line-height 24px
        font-size 10px
        color rgba(255,255,255, 0.4)
    .content-right
      flex 0 0 105px
      width 105px
      .pay
        height 48px
        line-height 48px
        text-align center
        font-size 12px
        color rgba(255,255,255, 0.4)
        font-weight 700
        &.not-enough
          background-color #2b333b
        &.enough
          background-color #00b43c
          color #fff
  .ball-wrap
    .ball
      position: fixed
      left: 32px
      bottom: 22px
      z-index: 100
      transition all 1s  cubic-bezier(0.49, -0.29, 0.75, 0.41)
      .inner
        width 16px
        height 16px
        border-radius 50%;
        background-color rgb(0, 160, 220)
        transition all 1s linear
</style>
