<template>
  <div class="container new_wrapper">
    <nuxt-link :to="{ name: 'index' }" class="button--grey back__btn new-back">
      戻る
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
      <button
        type="submit"
        class="button--green send__btn"
        @click.prevent="postWord()"
      >
        送信
      </button>
    </form>
  </div>
</template>

<script>
const contentful = require('contentful-management')
const client = contentful.createClient({
  accessToken: process.env.CTF_FOR_MY_APP_TOKEN
})

export default {
  data() {
    return {
      formData: {
        word: null,
        meaning: null,
        appearanceCount: null,
        correctCount: null,
        correctRate: null
      }
    }
  },
  methods: {
    async postWord() {
      const mySpace = await client.getSpace(process.env.CTF_SPACE_ID)
      const myEnvironment = await mySpace.getEnvironment(
        process.env.CTF_ENVIRONMENT_ID
      )
      const res = await myEnvironment.createEntry(
        process.env.CTF_CONTENT_TYPE_ID,
        {
          fields: {
            word: {
              'en-US': this.formData.word
            },
            meaning: {
              'en-US': this.formData.meaning
            },
            appearanceCount: {
              'en-US': +this.formData.appearanceCount
            },
            correctCount: {
              'en-US': +this.formData.correctCount
            },
            correctRate: {
              'en-US': +this.formData.correctRate
            }
          }
        }
      )
      await res.publish()
      // if (!res || res.status !== 200) {
      //   console.error(res)
      //   return
      // }
      console.log(res)
      this.formData.word = await null
      this.formData.meaning = await null
      this.formData.appearanceCount = await null
      this.formData.correctCount = await null
      this.formData.correctRate = await null
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
  width: 200px;
}
</style>
