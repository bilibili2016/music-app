<template>
  <div class="singer">
    <list-view :singerData="singerList" @slect="selectSinger"></list-view>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
import axios from 'axios'
import Singer from 'common/js/singer'
import listView from 'base/listView/listView'
import { mapMutations } from 'vuex'
const HOT_NAME = '热门'
const HOT_SINGER_LEN = 10
export default{
  data () {
    return {
      singerList: []
    }
  },
  components: {
    listView
  },
  created () {
    this._getSingerList()
  },
  methods: {
    selectSinger (singer) {
      this.$router.push({
        path: `/singer/${singer.id}`
      })
      this.setSinger(singer)
    },
    _getSingerList () {
      axios.get('../../../static/mock/singerList.json')
        .then((res) => {
          if (res.status === 200 && res.data) {
            const result = res.data.data
            this.singerList = this.initSingerData(result.list)
          }
        })
    },
    initSingerData (list) {
      let map = {
        hot: {
          title: HOT_NAME,
          items: []
        }
      }
      list.forEach((item, index) => {
        if (index < HOT_SINGER_LEN) {
          map.hot.items.push(new Singer({
            id: item.Fsinger_mid,
            name: item.Fsinger_name
          }))
        }

        const key = item.Findex
        if (!map[key]) {
          map[key] = {
            title: key,
            items: []
          }
        }
        map[key].items.push(new Singer({
          id: item.Fsinger_mid,
          name: item.Fsinger_name
        }))
      })

      let ret = []
      let hot = []
      for (let key in map) {
        let val = map[key]
        if (val.title === HOT_NAME) {
          hot.push(val)
        } else if (map[key].title.match(/[a-zA-Z]/)) {
          ret.push(val)
        }
      }
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0)
      })
      return hot.concat(ret)
    },
    ...mapMutations({
      setSinger: 'SET_SINGER'
    })
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  .singer
    position: fixed
    top: 88px
    bottom: 0
    width: 100%
</style>
