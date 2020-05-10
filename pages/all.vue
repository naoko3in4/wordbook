<template>
  <div class="container all__wrapper">
    <h2 class="list-title">登録単語一覧</h2>
    <div class="all_word_list__wrapper">
      <ul
        v-for="content in contents"
        :key="content.sys.id"
        class="all_word-list"
      >
        <li class="word__wrapper">
          <span class="registerd_word">{{ content.fields.word }}</span
          ><span class="registerd_meaning">:{{ content.fields.meaning }}</span>
        </li>
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
  max-width: 350px;
  margin-bottom: 30px;
  background: #fff;
  padding: 20px;
  padding-bottom: 35px;
  border-radius: 9px;
}

.all_word-list {
  text-align: left;
}

.list-title {
  font-weight: 700;
  font-size: 20px;
  color: #35495e;
  letter-spacing: 1px;
  margin-bottom: 15px;
}

.word__wrapper {
  line-height: 30px;
  border-bottom: 1px solid #d6d7d8;
}

.registerd_word {
  font-weight: 700;
  color: #0a6095;
}

.registerd_meaning {
  font-size: 14px;
}
</style>
