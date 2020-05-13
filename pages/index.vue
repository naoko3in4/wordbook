<template>
  <div class="container index__wrapper">
    <div>
      <!-- <logo /> -->
      <h1 class="title">
        wordbook
      </h1>
      <div v-if="contentsCreatedAtYesterday.length > 0">
        <div>
          <app-button
            color="blue"
            text="テストスタート"
            :class="{ isStarted: isStartActive }"
            @click="testStart()"
          />
        </div>

        <div v-if="isShown">
          <ul
            v-for="(content, index) in contentsCreatedAtYesterday"
            :key="content.sys.id"
            class="max-w-sm rounded overflow-hidden shadow-lg content__wrapper"
          >
            <li class="px-6 py-4" @click="deleteWord(index)">
              <p class="font-bold text-xl mb-2">
                {{ content.fields.word }}<span class="delete-btn">✗</span>
              </p>
            </li>
          </ul>
        </div>
      </div>
      <div v-else>
        <p class="no-word-text">回答できる単語はありません</p>
      </div>

      <div class="links">
        <nuxt-link :to="{ name: 'new' }">
          <app-button color="green" size="x_small" text="新しく登録する" />
        </nuxt-link>
        <nuxt-link :to="{ name: 'posts' }">
          <app-button color="grey" text="登録単語一覧" />
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
import AppButton from '@/components/AppButton.vue'
import contentfulClient from '@/plugins/contentful'
import moment from '@/plugins/moment'

export default {
  components: {
    AppButton
  },
  asyncData({ env }) {
    return contentfulClient
      .getEntries()
      .then((entries) => {
        console.log(entries.items)
        return {
          contents: entries.items
        }
      })
      .catch(console.error)
  },
  data() {
    return {
      isStartActive: false,
      isShown: false,
      contentsCreatedAtYesterday: []
    }
  },
  computed: {
    //
  },
  mounted() {
    const dateYesterday = moment().subtract(1, 'days')._d
    const contentsBoolean = this.contents.filter((content) => {
      const contentDate = moment(content.sys.createdAt)._d
      return moment(contentDate).isSame(dateYesterday, 'day')
    })
    const yesterdayContents = contentsBoolean
    this.contentsCreatedAtYesterday = yesterdayContents
    console.log('contentsCreatedAtYesterday', this.contentsCreatedAtYesterday)
  },
  methods: {
    testStart() {
      this.isStartActive = !this.isStartActive
      this.isShown = !this.isShown
    },
    deleteWord(index) {
      this.contentsCreatedAtYesterday.splice(index, 1)
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
/* Quicksand font */
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@700&display=swap');

.index__wrapper {
  display: flex;
  justify-content: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 65px;
  color: #35495e;
  letter-spacing: 1px;
}

.content__wrapper {
  margin: 0 auto;
}

.links {
  padding-top: 15px;
  display: flex;
  justify-content: space-around;
}

.isStarted {
  display: none;
}

.delete-btn {
  margin-left: 10px;
  color: #0a6095;
  font-size: 14px;
  font-weight: 700;
}

.no-word-text {
  margin-bottom: 10px;
  color: #0a6095;
  font-weight: 700;
}
</style>
