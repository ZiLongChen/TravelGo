<template>
  <div>
    <div class="search">
        <input v-model="keyword" class="search-input" type="text" placeholder="输入城市名或拼音"/>
    </div>
    <div class="search-content" ref="search" v-show="keyword">
        <ul>
          <li class="search-item border-bottom"
              v-for="item of list"
              :key="item.id"
              @click="handleCityClick(item.name)">
            {{item.name}}
          </li>
          <li class="search-item border-bottom" v-show="hasNoData">
            没有找到匹配数据
          </li>
        </ul>
    </div>
  </div>
</template>
<script>
import Bscroll from 'better-scroll'
import { mapMutations } from 'vuex'
export default {
  name: 'CitySearch',
  props: {
    tranCities: Object
  },
  data () {
    return {
      keyword: '',
      list: [], // 存储了城市关键词
      timer: null
    }
  },
  computed: {
    hasNoData () {
      return !this.list.length
    }
  },
  watch: {
    keyword () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      if (!this.keyword) {
        this.list = []
        return
      }
      this.timer = setTimeout(() => {
        const result = []
        for (let i in this.tranCities) {
          this.tranCities[i].forEach((value) => {
            if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
              result.push(value)
            }
          })
        }
        this.list = result
      }, 100)
    }
  },
  methods: {
    handleCityClick (city) {
      // 优化this.$store.commit('changeCityxxx', city)跟this.changeCityxxx(city)等同，只是后一方法引入了mapMUtations
      this.changeCityxxx(city)
      this.$router.push('/')
    },
    ...mapMutations(['changeCityxxx']) // 将mutations映射到一个名为changeCityxxx的方法中
  },
  mounted () {
    this.scroll = new Bscroll(this.$refs.search)
  }
}
</script>
<style scoped lang="stylus">
  @import '~styles/varibles.styl'
  .search
    height: .72rem
    padding: 0 .1rem
    background: $xxxbgColor
    .search-input
      box-sizing: border-box
      width: 100%
      height: .62rem
      padding: 0 .1rem
      line-height: .62rem
      text-align: center
      border-radius: .06rem
      color: #666
  .search-content
    overflow: hidden
    position: absolute
    top: 1.58rem
    left: 0
    right: 0
    bottom: 0
    background: #eee
    z-index: 1
    .search-item
      line-height: .62rem
      padding-left: .2rem
      color: #666
      background: #ffffff
</style>
