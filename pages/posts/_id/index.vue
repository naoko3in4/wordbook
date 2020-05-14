<template>
  <div class="container new_wrapper">
    <h1>編集ページです</h1>
    <!-- <p>{{ $route.params.id }}</p> -->
    <nuxt-link :to="{ name: 'index' }">
      <app-button color="grey" text="戻る" class="new-back" />
    </nuxt-link>
    <form class="register__container">
      <label for="単語">
        <input
          v-model="formData.word"
          type="text"
          placeholder="単語を入力"
          class="write__item"
        />
      </label>
      <label for="意味">
        <input
          v-model="formData.meaning"
          type="text"
          placeholder="意味を入力"
          class="write__item"
        />
      </label>
      <!-- 今後活用するかも -->
      <!-- <label for="出題回数">
        <input
          v-model="formData.appearanceCount"
          type="number"
          placeholder="出題回数"
        />
      </label>
      <label for="正答回数">
        <input
          v-model="formData.correctCount"
          type="number"
          placeholder="正答回数"
        />
      </label>
      <label for="正解率">
        <input
          v-model="formData.correctRate"
          type="number"
          placeholder="正解率"
        />
      </label> -->
      <app-button
        color="green"
        size="large"
        text="送信"
        class="send__btn"
        @click.prevent="postWord()"
      />
    </form>
  </div>
</template>

<script>
import AppButton from '@/components/AppButton.vue'

const contentful = require('contentful-management')
const client = contentful.createClient({
  accessToken: process.env.CTF_FOR_MY_APP_TOKEN
})

export default {
  components: {
    AppButton
  },
  async asyncData({ route }) {
    const entryId = route.params.id
    const mySpace = await client.getSpace(process.env.CTF_SPACE_ID)
    const entry = await mySpace.getEntry(entryId)
    return {
      entry
    }
  },
  data() {
    return {
      testParam: null,
      formData: {
        word: null,
        meaning: null,
        appearanceCount: null,
        correctCount: null,
        correctRate: null
      }
    }
  },
  created() {
    this.formData.word = this.entry.fields.word['en-US']
    this.formData.meaning = this.entry.fields.meaning['en-US']
  },
  methods: {
    async postWord() {
      this.entry.fields.word = {
        'en-US': this.formData.word
      }
      this.entry.fields.meaning = {
        'en-US': this.formData.meaning
      }
      const updateEntry = await this.entry.update()
      await updateEntry.publish()

      console.log(this.entry)

      Object.keys(this.formData).map((key) => {
        this.formData[key] = null
      })
    }
  }
}
</script>

<style>
.new_wrapper {
  padding-top: 200px;
}

.register__container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}

.new-back {
  margin-bottom: 20px;
}

.write__item {
  padding: 5px;
  margin-bottom: 20px;
  border-radius: 7px;
}

.send__btn {
  margin: 0 auto;
}
</style>
