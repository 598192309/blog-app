<!-- 文件路径：src/views/HomeView.vue -->
<script setup>
import { ref, computed } from 'vue'
import BlogCard from '../components/BlogCard.vue'
import CategoryFilter from '../components/CategoryFilter.vue'

// 文章数据
const articles = ref([
  { id: 1, title: 'Vue3 入门完全指南', summary: '从零开始学习 Vue3 组合式 API', date: '2024-05-10', category: 'Vue' },
  { id: 2, title: 'JS 异步编程详解', summary: '搞懂 Promise 和 async/await', date: '2024-05-08', category: 'JavaScript' },
  { id: 3, title: 'CSS Grid 布局实战', summary: '用 Grid 实现响应式布局', date: '2024-05-05', category: 'CSS' },
  { id: 4, title: 'Vue3 响应式原理', summary: '深入理解 ref 和 reactive', date: '2024-05-03', category: 'Vue' },
  { id: 5, title: 'Flexbox 完全指南', summary: '一文学会弹性布局', date: '2024-05-01', category: 'CSS' },
])

// 提取所有分类（去重）
const categories = computed(() => {
  const cats = articles.value.map(a => a.category)
  return ['全部', ...new Set(cats)]
})

// 当前选中的分类
const activeCategory = ref('全部')

// 根据分类过滤文章
const filteredArticles = computed(() => {
  if (activeCategory.value === '全部') return articles.value
  return articles.value.filter(a => a.category === activeCategory.value)
})

// 处理子组件发来的分类切换事件
function handleCategoryChange(cat) {
  activeCategory.value = cat
}
</script>

<template>
  <div class="home">
    <h2 class="section-title">最新文章</h2>

    <!-- 分类筛选组件 -->
    <CategoryFilter
      :categories="categories"
      :active-category="activeCategory"
      @update-category="handleCategoryChange"
    />

    <!-- 空状态 -->
    <p v-if="filteredArticles.length === 0" class="empty-tip">
      该分类下暂无文章
    </p>

    <!-- 文章卡片列表 -->
    <div v-else class="article-grid">
      <BlogCard
        v-for="article in filteredArticles"
        :key="article.id"
        :title="article.title"
        :summary="article.summary"
        :date="article.date"
        :category="article.category"
      />
    </div>
  </div>
</template>

<style scoped>
.home { max-width: 960px; margin: 0 auto; padding: 20px; }
.section-title { font-size: 24px; margin-bottom: 20px; }
.article-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 24px;
  margin-top: 20px;
}
.empty-tip { text-align: center; color: #999; padding: 60px 0; }
</style>