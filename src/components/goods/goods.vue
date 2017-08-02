<template>
    <div class="goods">
      <div class="menu-wrapper" ref="menu-scroll">
        <ul>
          <li class="menu-item" v-for="(item, index) in goods" :class="{'current': index === currentIndex}" @click="selectMenu(index, $event)">            
            <span class="text border-1px">
              <i class="icon" :class="classMap[item.type]" v-if="item.type>0"></i>{{item.name}}              
            </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foods-scroll">
        <ul>
          <li class="foods-list js-foods-list" v-for="item in goods">
            <h2 class="title">{{item.name}}</h2>
            <ul>
              <li class="food-item border-1px" v-for="food in item.foods">
                <div class="thumb"><img :src="food.icon" width="57" height="57"></div>
                <div class="content">
                  <h3 class="name">{{food.name}}</h3>
                  <p v-show="food.description" class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥<span class="num">{{food.price}}</span></span><span v-show="food.oldPrice" class="old">￥<span class="num">{{food.oldPrice}}</span></span>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
  import BScroll from 'better-scroll'

  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    computed: {
      currentIndex () {
        for (var i = 0; i < this.listHeight.length - 1; i++) {
          let top = this.listHeight[i]
          let bottom = this.listHeight[i + 1]

          if (this.scrollY < bottom && this.scrollY >= top) {
            return i
          }
        }
        return 0
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
      this.axios.get('/api/goods')
        .then((response) => {
          response = response.data
          if (response.errno === ERR_OK) {
            this.goods = response.data
            this.$nextTick(function () {
              this._initScroll()
              this._calculateHeight()
            })
          }
        })
    },
    methods: {
      _initScroll () {
        this.$menuScroll = new BScroll(this.$refs['menu-scroll'], { click: true })
        this.$foodsScroll = new BScroll(this.$refs['foods-scroll'], { probeType: 3 })
        this.$foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calculateHeight () {
        let els = this.$refs['foods-scroll'].getElementsByClassName('js-foods-list')
        let height = 0
        this.listHeight.push(height)
        for (var i = 0; i < els.length; i++) {
          var el = els[i]
          height += el.clientHeight
          this.listHeight.push(height)
        }
      },
      selectMenu (index, event) {
        if (!event._constructed) return

        let el = this.$refs['foods-scroll'].getElementsByClassName('js-foods-list')[index]
        this.$foodsScroll.scrollToElement(el, 300)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"

  .goods
    display flex
    position absolute
    top 174px
    bottom 40px
    width 100%
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background-color #f3f5f7
      .menu-item
        display table
        padding: 0 12px
        width 56px
        height 54px
        line-height: 14px
        .icon
          display inline-block
          vertical-align top
          width: 12px
          height 12px
          margin-right: 2px
          background-size 12px 12px
          background-repeat no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          display table-cell
          width 56px
          vertical-align middle
          border-1px(rgba(7,17,27,0.1))
          font-size 12px
    .foods-wrapper
      flex 1
      .foods-list
        .title
          padding-left 14px
          height 28px
          line-height 28px
          border-left 2px solid #d9dde1
          font-size 12px
          color rgb(147,153,159)
          background-color #f3f5f7
      .food-item
        display flex
        margin: 18px 18px 0
        padding-bottom: 18px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
        .thumb
          flex 0 0 57px
          margin-right 10px
          width 57px
          
          img 
            display block
        .content
          flex 1
          .name
            margin 2px 0px 8px
            line-height 14px
            font-size 14px
            color rgb(7,17,27)
          .desc
            margin-bottom 8px
            line-height 10px
            font-size 10px            
            color rgb(147,153,159)
          .extra            
            margin-bottom 8px
            line-height 10px
            font-size 10px            
            color rgb(147,153,159)
            .count
              margin-right 12px
          .price
            font-size 10px
            line-height 24px
            .now
              color rgb(240, 20, 20)
              .num
                margin-right 12px
                font-size 14px
                font-weight 700
            .old 
              color rgb(147,153,159)
              text-decoration line-through
              .num
                font-weight 700
          
</style>
