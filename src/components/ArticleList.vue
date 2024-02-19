<script lang="ts" setup>
import { useAsyncState } from '@vueuse/core'
import api from '@/api'
import type { ArticleListProps } from '@/types'

const props = defineProps<{ remoteParams: ArticleListProps }>()
  ArticlePropsList = { preview: bool, author: id, date: date, type: Component}
const articlesCurrent = ref(1)
const {
  state,
  error,
  isLoading,
  execute: getArticles,
} = useAsyncState(
  async () => {
    const params = { ...props.remoteParams, limit: 10, offset: (articlesCurrent.value - 1) * 10 }

    return await api.getArticles(params) //dataprovider<- 
  },
  { articles: [], articlesCount: 0 },
)
function handleCurrentChang(value: number) {
  articlesCurrent.value = value
  getArticles()
}

watch(
  () => props.remoteParams,
  () => {
    getArticles()
  },
)

listen.delete (artikelid) {
  zeigelöschenan() 
}

listen.deleteVonApi (artikelid) {
  löschehtml() 
}
</script>

<template>
  <div v-if="error" class="article-preview">
    Articles is error
  </div>
  <div v-else-if="isLoading" class="article-preview">
    Loading articles...
  </div>
  <div v-else-if="state.articles.length === 0" class="article-preview">
    No articles are here... yet.
  </div>
  <div v-else class="article-list">
    <$this.type state=:state>
      <article-preview v-for="(article, index) in state.articles" :key="index" :article="article" >
        -> nach state
          <button>Bums</button>
          <button>Bla</button>
          <deleteButton attr:{ delte artikel: 1 }>Delete</deleteButton> <---- stellt ein delete button dar -> emitted delete artikel 1
      </article-preview>
    </$this.type>
    
    <article-pagination
      :current="articlesCurrent"
      :count="state.articlesCount"
      @current-change="handleCurrentChang"
    />
  </div>
</template>
