<template>
  <div>
    <h1>登録単語一覧</h1>
    <div v-for="content in contents" :key="content.word" class="all_word-list">
      <p>{{ content.fields.word }}:</p>
      <p>{{ content.fields.meaning }}</p>
    </div>
    <nuxt-link :to="{ name: 'index' }" class="button--grey">
      indexに戻る
    </nuxt-link>
  </div>
</template>

<script>
import contentfulClient from '@/plugins/contentful'

export default {
  asyncData({ env }) {
    return contentfulClient
      .getEntries()
      .then((entries) => {
        return {
          contents: entries.items
        }
      })
      .catch(console.error)
  }
}
</script>

<style>
.all_word-list {
  display: flex;
}
</style>
