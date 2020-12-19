<template>
  <div class="search-history">
    <van-cell title="搜索历史">
      <template v-if="isDeleteShow">
        <span @click="$emit('clear-search-history')">全部删除</span>
        &nbsp; &nbsp;
        <span @click="isDeleteShow = false">完成</span>
      </template>
      <van-icon name="delete" @click="isDeleteShow = true" v-else />
    </van-cell>
    <van-cell :title="item" v-for="(item, index) in searchHistory" :key="index" @click="onHistoryClick(item, index)">
      <van-icon
        v-show="isDeleteShow"
        name="close"
        @click="searchHistory.splice(index, 1)"
      />
    </van-cell>
  </div>
</template>

<script>
export default {
  name: 'SearchHistory',
  components: {},
  props: {
    searchHistory: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      isDeleteShow: false
    }
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {
    onHistoryClick(item, index) {
      // 如果是删除状态，则执行删除操作
      if (this.isDeleteShow) {
        this.searchHistory.splice(index, 1)
      } else {
        // 否则执行搜索操作
        this.$emit('search', item)
      }
    }
  }
}
</script>

<style scoped lang="less"></style>
