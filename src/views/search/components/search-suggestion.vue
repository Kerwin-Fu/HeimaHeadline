<template>
  <div class="search-suggestion">
    <van-cell v-for="(text, index) in suggestions" :key="index" icon="search" @click="$emit('search', text)">
      <div v-html="highlight(text)" slot="title"></div>
    </van-cell>
  </div>
</template>

<script>
import { getSearchSuggestion } from '@/api/search.js'
import { debounce } from 'lodash'
export default {
  name: 'SearchSuggestion',
  components: {},
  props: {
    searchText: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      suggestions: ''
    }
  },
  computed: {},
  watch: {
    searchText: {
      // 监视的处理函数
      handler: debounce(function(val) {
        this.loadSearchSuggestion(val)
      }, 300),
      // 首次监视触发
      immediate: true
    }
  },
  created() {},
  mounted() {},
  methods: {
    async loadSearchSuggestion(q) {
      try {
        const { data } = await getSearchSuggestion(q)
        this.suggestions = data.data.options
        console.log(this.searchText)
      } catch {
        this.$toast('获取失败')
      }
    },
    highlight(text) {
      const reg = new RegExp(this.searchText, 'gi')
      return text.replace(reg, `<span class="active">${this.searchText}</span>`)
    }
  }
}
</script>

<style lang="less" scoped>
.search-suggestion {
  /deep/ span.active {
    color: #3296fa;
  }
}
</style>
