<!-- 文件路径：src/views/PostView.vue -->
<script setup>
import { ref, computed } from 'vue'
import { useRoute, RouterLink } from 'vue-router'

const route = useRoute()
const id = Number(route.params.id)

// 所有文章数据（后续章节会抽离到公共模块）
const articles = ref([
  {
    id: 1,
    title: 'Vue3 入门完全指南',
    summary: '从零开始学习 Vue3 组合式 API',
    category: 'Vue',
    date: '2024-05-10',
    content: `
      <h2>为什么学 Vue3？</h2>
      <p>Vue3 是目前最流行的前端框架之一，它易学、灵活、性能出色。</p>
      <h2>组合式 API 的优势</h2>
      <p>相比 Vue2 的选项式 API，组合式 API 让逻辑复用变得简单，代码组织更加灵活。</p>
      <h2>ref 与 reactive</h2>
      <p>ref 适合基本类型数据，reactive 适合对象和数组。但实际开发中推荐统一使用 ref。</p>
    `
  },
  {
    id: 2,
    title: 'JS 异步编程详解',
    summary: '搞懂 Promise 和 async/await',
    category: 'JavaScript',
    date: '2024-05-08',
    content: `
      <h2>什么是异步？</h2>
      <p>JS 是单线程的，异步操作可以让主线程不阻塞，同时处理网络请求、定时器等任务。</p>
      <h2>Promise 的核心概念</h2>
      <p>Promise 有三种状态：pending（进行中）、fulfilled（已成功）、rejected（已失败）。</p>
    `
  }
])

// 根据 ID 查找文章
const article = computed(() => {
  return articles.value.find(a => a.id === id)
})

watch(articles, (newArticles,oldArticles) => {
  if (!newArticles.value) {
    console.warn(`找不到 ID 为 ${id} 的文章`)
  }
})
</script>

<template>
  <div class="post-view">
    <!-- 文章不存在 -->
    <div v-if="!article" class="not-found">
      <h2>文章不存在</h2>
      <p>找不到 ID 为 {{ id }} 的文章</p>
      <RouterLink to="/">返回首页</RouterLink>
    </div>

    <!-- 文章存在，展示完整内容 -->
    <article v-else>
      <span class="category-tag">{{ article.category }}</span>
      <h1>{{ article.title }}</h1>
      <time>{{ article.date }}</time>
      <!-- v-html 渲染 HTML 字符串 -->
      <div class="content" v-html="article.content"></div>
      <RouterLink to="/" class="back-link">← 返回首页</RouterLink>
    </article>
  </div>
</template>

<style scoped>
.post-view {
  max-width: 720px;
  margin: 40px auto;
  padding: 0 20px;
}

.category-tag {
  display: inline-block;
  padding: 4px 12px;
  background: #e8f5e9;
  color: #42b883;
  border-radius: 12px;
  font-size: 13px;
  margin-bottom: 12px;
}

h1 {
  font-size: 32px;
  margin-bottom: 12px;
  line-height: 1.4;
}

time {
  display: block;
  color: #999;
  font-size: 14px;
  margin-bottom: 30px;
}

.content {
  line-height: 1.8;
  font-size: 16px;
  color: #333;
}

.content :deep(h2) {
  margin: 24px 0 12px;
  font-size: 22px;
}

.content :deep(p) {
  margin-bottom: 12px;
}

.back-link {
  display: inline-block;
  margin-top: 40px;
  color: #42b883;
  text-decoration: none;
}

.not-found {
  text-align: center;
  padding: 60px 0;
}
</style>