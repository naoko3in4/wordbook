<template>
  <div class="container">
    <div>
      <!-- <logo /> -->
      <h1 class="title">
        wordbook
      </h1>
      <h2 class="subtitle">
        今日のテスト
      </h2>
      <div>
        <button @click="testStart()">テストスタート</button>
      </div>
      <p>{{ yesterday }}</p>
      <div v-for="content in contents" :key="content.word">
        <p v-if="content.sys.createdAt.includes(yesterday)">
          {{ content.fields.word }}
        </p>
        <p v-if="content.sys.createdAt.includes(yesterday)">
          {{ content.fields.meaning }}
        </p>
      </div>

      <div class="links">
        <nuxt-link :to="{ name: 'new' }" class="button--green">
          新しく登録する
        </nuxt-link>
        <nuxt-link :to="{ name: 'all' }" class="button--grey">
          登録単語一覧
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
// import Logo from '@/components/Logo.vue'
import contentfulClient from '@/plugins/contentful'
import moment from '@/plugins/moment'

export default {
  components: {
    // Logo
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
      yesterday: null
    }
  },
  methods: {
    testStart() {
      // yesterday (English)
      const yesterdayStrLong = moment().subtract(1, 'days')._d
      const yesterdayStr = yesterdayStrLong.toString().substring(4, 15)
      const yesterdayMonth = yesterdayStr.toString().substring(0, 3)
      const months = {
        Jan: '01',
        Feb: '02',
        Mar: '03',
        Apr: '04',
        May: '05',
        Jun: '06',
        Jul: '07',
        Aug: '08',
        Sep: '09',
        Oct: '10',
        Nov: '11',
        Dec: '12'
      }
      for (const [key, value] of Object.entries(months)) {
        if (key === yesterdayMonth) {
          const formatedYesterday =
            yesterdayStr.substring(7, 11) +
            '-' +
            value +
            '-' +
            yesterdayStr.substring(4, 6)
          this.yesterday = formatedYesterday
          console.log(this.yesterday)
        }
      }
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
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
