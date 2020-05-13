<template>
  <div class="container all__wrapper">
    <h2 class="list-title">登録単語一覧</h2>
    <div class="all_word_list__wrapper">
      <div
        v-for="content in contents"
        :key="content.sys.id"
        class="all_word-list"
      >
        <div class="word__wrapper">
          <span class="registerd_word">{{ content.fields.word }}</span
          ><span class="registerd_meaning">:{{ content.fields.meaning }}</span>
        </div>
        <div class="edit-btn" @click="goToEditPage(content.sys.id)">
          編集
        </div>
      </div>
    </div>

    <nuxt-link :to="{ name: 'index' }">
      <app-buttton color="grey" text="戻る" />
    </nuxt-link>
  </div>
</template>

<script>
import contentfulClient from '@/plugins/contentful'
import AppButtton from '@/components/AppButton.vue'

export default {
  components: {
    AppButtton
  },
  asyncData({ env }) {
    return contentfulClient
      .getEntries()
      .then((entries) => {
        return {
          contents: entries.items
        }
      })
      .catch(console.error)
  },
  methods: {
    goToEditPage(id) {
      console.log('id', id)
      this.$router.push({
        path: `posts/${id}`
      })
    }
  }
}
</script>

<style>
.all__wrapper {
  padding-top: 80px;
}

.all_word-list {
  border-bottom: 1px solid #d6d7d8;
}

.all_word_list__wrapper {
  margin: 0 auto;
  max-width: 350px;
  max-height: 450px;
  overflow: scroll;
  margin-bottom: 30px;
  background: #fff;
  padding: 20px;
  padding-bottom: 35px;
  border-radius: 9px;
}

.all_word-list {
  /* text-align: left; */
  display: flex;
  justify-content: space-between;
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
}

.registerd_word {
  font-weight: 700;
  color: #0a6095;
}

.registerd_meaning {
  font-size: 14px;
}

.edit-btn {
  width: 40px;
  font-size: 11px;
  line-height: 30px;
  border-radius: 5px;
  text-align: center;
  color: #b9babb;
  border: 1px solid #b9babb;
}
</style>
