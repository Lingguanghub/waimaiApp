<template>
  <div class="seller" ref="seller">
    <div class="seller_content">
      <div class="overview">
        <h1 class="title">{{ seller.name }}</h1>
        <div class="desc border-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{ seller.ratingCount }})</span>
          <span class="text">月售{{ seller.sellCount }}单</span>
        </div>
        <!--========================================================-->
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span>{{ seller.minPrice }}</span>分钟
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span>{{ seller.deliveryPrice }}</span>元
            </div>
          </li>
          <!--=====================================-->
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span>{{ seller.deliveryTime }}</span>分钟
            </div>
          </li>
          <!--============================================-->
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active': favorite}"></span>
          <span class="text">{{ favoriteText }}</span>
        </div>
      </div>
      <split></split>
      <!--============================================================-->
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{ seller.bulletin }}</p>
        </div>
        <!--===============================================-->
        <ul v-if="seller.supports" class="supports">
          <li class="supports_item" v-for="(item,index) in seller.supports" :key="index">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{ seller.supports[index].description }}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic_list" ref="picList">
            <li class="pic_item" v-for="(pic,index) in seller.pics" :key="index">
              <img :src="pic" width="120" height="90" />
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <!--=========================================-->
      <div class="info">
        <h1 class="title border-1px">商家信息</h1>
        <ul>
          <li class="info_item" v-for="(info,index) in seller.infos" :key="index">{{ info }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import star from 'components/star/star'
import split from 'components/split/split'
import {saveToLocal, loadFromLocal} from 'common/js/store'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      favorite: (() => {
        return loadFromLocal(this.seller.id, 'favorite', this.favorite)
      })()
    }
  },
  created () {
    this.classMap = [ 'decrease', 'discount', 'special', 'invoice', 'guarantee' ]
    this.$nextTick(() => {
      this._initScroll()
      this._initPics()
    })
  },
  methods: {
    toggleFavorite () {
      this.favorite = !this.favorite
      saveToLocal(this.seller.id, 'favorite', this.favorite)
    },
    _initScroll () {
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.seller, {
          chick: true
        })
      } else {
        this.scroll.refresh()
      }
    },
    _initPics () {
      if (this.seller.pics) {
        let picWidth = 120
        let margin = 6
        let width = (picWidth + margin) *
        this.seller.pics.length - margin
        this.$refs.picList.style.width = width + 'px'
        this.$nextTick(function () {
          if (!this.picScroll) {
            this.picScroll = new BScroll(this.$refs.picWrapper, { scrollX: true, eventPassthrough: 'vertical' })
          } else {
            this.picScroll.refresh()
          }
        })
      }
    }
  },
  computed: {
    favoriteText () {
      return this.favorite ? '已收藏' : '收藏'
    }
  },
  components: {
    star,
    split
  }
}
</script>

<style lang="stylus">
@import "../../common/stylus/mixin.styl"
.seller
  position: absolute
  top: 174px
  bottom: 0
  left: 0
  width: 100%
  overflow: hidden
  .overview
    position: relative
    padding: 18px
    .title
      margin-bottom: 8px
      line-height: 14px
      color: rgb(7,17,27)
      font-size: 14px
    .desc
      padding-bottom: 18px
      border-1px(rgba(7,17,27,.1))
      font-size: 0
      .star
        display: inline-block
        margin-right: 8px
        vertical-align: middle
      .text
        display: inline-block
        margin-right: 12px
        line-height: 18px
        font-size: 10px
        color: rgb(77,85,93)
        vertical-align: middle
    .remark
      display: flex
      padding-top: 18px
      .block
        flex: 1
        text-align: center
        border-right: 1px solid rgba(7,17,27,.1)
        &:last-child
          border: none
        h2
          font-size: 10px
          color: rgb(147,153,159)
          line-height: 10px
        .content
          line-height: 24px
          font-weight: 200
          font-size: 20px
          color: rgb(7,17,27)
          margin-top: 4px
    .favorite
      position: absolute
      width: 50px
      right: 11px
      top: 18px
      text-align: center
      .icon-favorite
        display: block
        margin-bottom: 4px
        line-height: 24px
        font-size: 24px
        color: #d4d6d9
        &.active
          color: rgb(240,20,20)
      .text
        font-size: 10px
        color: rgb(77,85,93)
        line-height: 10px
  .bulletin
    padding: 18px 18px 0 18px
    .title
      margin-bottom: 8px
      line-height: 14px
      color: rgb(7,17,27)
      font-size: 14px
    .content-wrapper
      padding: 0 12px 6px 12px
      border-1px(rgba(7,17,27,.1))
      .content
        line-height: 24px
        color: rgb(240,20,20)
        font-weight: 200
        font-size: 12px
    .supports
      .supports_item
        padding: 16px 12px
        border-1px(rgba(7,17,27,.1))
        font-size: 0
        &:last-child
          border: none
        .icon
          display: inline-block
          width: 16px
          height: 16px
          vertical-align: middle
          margin-right: 6px
          background-size: 16px 16px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_4')
          &.discount
            bg-image('discount_4')
          &.special
            bg-image('special_4')
          &.invoice
            bg-image('invoice_4')
          &.guarantee
            bg-image('guarantee_4')
        .text
            line-height: 16px
            font-size: 12px
            font-weight: 200
            color: rgb(7,17,27)
            vertical-align: middle
  .pics
    padding: 18px
    .title
      margin-bottom: 12px
      line-height: 14px
      color: rgb(7,17,27)
      font-size: 14px
    .pic-wrapper
      width: 100%
      overflow: hidden
      white-space: nowrap
      .pic_list
        font-size: 0
        .pic_item
          display: inline-block
          margin-right: 6px
          width: 120px
          height: 90px
          &:last-child
            margin-right: 0
  .info
    padding: 18px
    color: rgb(7,17,27)
    .title
      padding-bottom: 12px
      line-height: 14px
      border-1px(rgba(7,17,27,.1))
      font-size: 14px
    .info_item
      padding: 16px 12px
      line-height: 16px
      border-1px(rgba(7,17,27,.1))
      font-size: 12px
      &:last-child
        border: none
</style>
