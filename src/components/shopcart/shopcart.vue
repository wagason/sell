<template>
    <div class="shopcart-wrap">
      <div class="content">
        <div class="content-left">
          <div class="icon-box">
            <div class="icon">
              <span class="icon-shopping_cart"></span>
            </div>
          </div>
          <div class="price">￥0</div>
          <div class="desc">另需配送费¥{{deliveryPrice}}元</div>
        </div>
        <div class="content-right"></div>
      </div>
    </div>
</template>

<script>
  export default {
    props: {
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
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
      .desc
        display inline-block
        vertical-align top
        margin 12px 0 0 12px
        line-height 24px
        font-size 10px
        color rgba(255,255,255, 0.4)
    .content-right
      flex 0 0 105px
      padding 0 8px
      width 105px
      font-weight 700
      font-size 16px
      line-height 48px
      color rgb(255,255,255,0.4) 
</style>
