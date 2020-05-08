<template>
  <div class="container all__wrapper">
    <h2>登録単語一覧</h2>
    <div class="all_word_list__wrapper">
      <ul
        v-for="content in contents"
        :key="content.sys.id"
        class="all_word-list"
      >
        <li>{{ content.fields.word }}:{{ content.fields.meaning }}</li>
      </ul>
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
.all__wrapper {
  margin-top: 20px;
}

.all_word_list__wrapper {
  margin: 0 auto;
  max-width: 200px;
}

.all_word-list {
  text-align: left;
}
</style>
