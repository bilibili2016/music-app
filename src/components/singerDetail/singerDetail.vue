<template>
  <transition name="slide">
    <music-list
      :songs="songs"
      :title="title"
      :bgImg="bgImg"
    ></music-list>
  </transition>
</template>

<script>
import { mapGetters } from 'vuex'
import { getSingerDetail } from 'api/singer'
import { ERR_OK } from 'api/config'
import { createSong } from 'api/song'
import musicList from 'components/musicList/musicList'
export default{
  data () {
    return {
      songs: []
    }
  },
  components: {
    musicList
  },
  computed: {
    title () {
      return this.singer.name
    },
    bgImg () {
      return this.singer.avatar
    },
    ...mapGetters([
      'singer'
    ])
  },
  created () {
    this._getSingerDetail()
  },
  methods: {
    _getSingerDetail () {
      getSingerDetail(this.singer)
        .then((res) => {
          if (res.code === ERR_OK) {
            this.songs = this._normallistSongs(res.data.list)
            console.log(this.songs)
          }
        })
    },
    _normallistSongs (list) {
      let ret = []
      list.forEach((item) => {
      //  es6对象的解构赋值
        let {musicData} = item
        if (musicData.songid && musicData.albumid) {
          ret.push(createSong(musicData))
        }
      })
      return ret
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "~common/stylus/variable"
  .singer-detail
    position: fixed
    z-index: 100
    top: 0
    left 0
    right: 0
    bottom: 0
    background: $color-background
  .slide-enter-active,.slide-leave-active
    transition: all 0.3s
  .slide-enter,.slide-leave-to
    transform: translate3d(100%, 0, 0)
</style>
