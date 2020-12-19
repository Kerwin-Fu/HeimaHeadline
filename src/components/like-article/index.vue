<template>
  <van-icon
    :class="{
      liked: value === 1,
    }"
    :name="value === 1 ? 'good-job' : 'good-job-o'"
    @click="onLike"
    :loading="loading"
  />
</template>

<script>
import { deleteLike, addLike } from '@/api/article.js'
export default {
  data() {
    return {
      loading: false
    }
  },

  props: {
    value: {
      type: Number,
      required: true
    },
    articleId: {
      type: [Number, String, Object],
      required: true
    }
  },

  created() {},

  methods: {
    async onLike() {
      // 两个作用：1、交互提示 2、防止网络慢用户连续不断的点击按钮请求
      this.$toast.loading({
        duration: 0, // 持续展示 toast
        message: '操作中...',
        forbidClick: true // 是否禁止背景点击
      })

      try {
        let status = -1
        // 如果已经点赞，则取消点赞
        if (this.value === 1) {
          await deleteLike(this.articleId)
        } else {
          // 否则添加点赞
          await addLike(this.articleId)
          status = 1
        }
        this.$emit('input', status)
        // 更新视图
        this.$toast.success(status === 1 ? '点赞成功' : '取消点赞')
      } catch (err) {
        console.log(err)
        this.$toast.fail('操作失败')
      }
    }
  }
}
</script>

<style scoped lang='less'>
.liked {
    color: #e5645f;
}
</style>
