<template>
  <scroll
    class="listview"
    :data="singerData"
    ref="listview"
  >
    <ul>
      <li
        class="list-group"
        v-for="group in singerData"
        :key="group.title"
        ref="listgroup"
      >
        <h2 class="list-group-title">{{group.title}}</h2>
        <ul>
          <li
            class="list-group-item"
            v-for="(item, index) in group.items"
            :key="index"
          >
            <img
              class="avatar"
              :src="item.avatar"
              v-lazy="item.avatar"
            >
            <span class="name">{{item.name}}</span>
          </li>
        </ul>
      </li>
    </ul>
    <div class="list-shortcut">
      <ul>
        <li
          class="item"
          v-for="(item, index) in shortcutList"
          :key="index"
          :data-index="index"
          @touchstart="onshortcutTouchStart"
        >
          {{item}}
        </li>
      </ul>
    </div>
    <div class="loading-container" v-show="!singerData.length">
      <loading></loading>
    </div>
  </scroll>
</template>

<script>
import Scroll from 'base/scroll/scroll'
import Loading from 'base/loading/loading'
import { getData } from 'common/js/dom'
export default {
  props: {
    singerData: {
      type: Array,
      default: []
    }
  },
  components: {
    Scroll,
    Loading
  },
  computed: {
    shortcutList () {
      return this.singerData.map((group) => {
        return group.title.substring(0, 1)
      })
    }
  },
  methods: {
    onshortcutTouchStart (e) {
      let anchorIndex = getData(e.target, 'index')
      this.$refs.listview.scrollToElement(this.$refs.listgroup[anchorIndex])
    }
  }
}
</script>


<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .listview
    position: relative
    width: 100%
    height: 100%
    overflow: hidden
    background: $color-background
    .list-group
      padding-bottom: 30px
      .list-group-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
      .list-group-item
        display: flex
        align-items: center
        padding: 20px 0 0 30px
        .avatar
          width: 50px
          height: 50px
          border-radius: 50%
        .name
          margin-left: 20px
          color: $color-text-l
          font-size: $font-size-medium
    .list-shortcut
      position: absolute
      z-index: 30
      right: 0
      top: 50%
      transform: translateY(-50%)
      width: 20px
      padding: 20px 0
      border-radius: 10px
      text-align: center
      background: $color-background-d
      font-family: Helvetica
      .item
        padding: 3px
        line-height: 1
        color: $color-text-l
        font-size: $font-size-small
        &.current
          color: $color-theme
    .list-fixed
      position: absolute
      top: 0
      left: 0
      width: 100%
      .fixed-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
    .loading-container
      position: absolute
      width: 100%
      top: 50%
      transform: translateY(-50%)
</style>