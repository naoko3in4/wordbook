<template>
  <div class="container index__wrapper">
    <div>
      <!-- <logo /> -->
      <h1 class="title">
        wordbook
      </h1>
      <div>
        <button
          class="button--blue"
          :class="{ isStarted: isStartActive }"
          @click="testStart()"
        >
          テストスタート
        </button>
      </div>
      <ul
        v-for="(content, index) in contents"
        :key="content.sys.id"
        class="max-w-sm rounded overflow-hidden shadow-lg content__wrapper"
      >
        <li
          v-if="content.sys.createdAt.includes(dateYesterday)"
          class="px-6 py-4"
        >
          <p class="font-bold text-xl mb-2">
            {{ content.fields.word }}<span @click="deleteWord(index)">✗</span>
          </p>
        </li>
      </ul>

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
      isStartActive: false,
      dateYesterday: null
    }
  },
  methods: {
    testStart() {
      const dateYesterdayStrLong = moment().subtract(1, 'days')._d
      const dateYesterdayStr = dateYesterdayStrLong.toString().substring(4, 15)
      const dateYesterdayMonth = dateYesterdayStr.toString().substring(0, 3)
      this.isStartActive = !this.isStartActive

      for (const [key, value] of Object.entries(months)) {
        if (key === dateYesterdayMonth) {
          const formatedDateYesterday =
            dateYesterdayStr.substring(7, 11) +
            '-' +
            value +
            '-' +
            dateYesterdayStr.substring(4, 6)
          this.dateYesterday = formatedDateYesterday
          console.log(this.dateYesterday)
        }
      }
    },
    deleteWord(index) {
      this.contents.splice(index, 1)
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
.index__wrapper {
  display: flex;
  justify-content: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 80px;
  color: #35495e;
  letter-spacing: 1px;
}

.content__wrapper {
  margin: 0 auto;
}

.links {
  padding-top: 15px;
}

.isStarted {
  display: none;
}
</style>
