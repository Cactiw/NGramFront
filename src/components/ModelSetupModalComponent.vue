<template>
  <n-modal style="max-width: 40%" preset="card" title="Параметры модели" v-model:show="show">
    <n-space vertical align="center">
      <n-h5>Всего слов: {{wordsCount}}</n-h5>
      <n-upload :action="defaultURL + `/api/addFile`" @finish="afterUpload" @before-upload="beforeUpload" @error="fileLoading = false">
        <n-button :loading="fileLoading" type="info">Добавить файл</n-button>
      </n-upload>
    </n-space>
  </n-modal>
</template>

<script>
import {inject, ref} from "vue";
import {NModal, NCard, NH5, NUpload, NButton, NSpace} from "naive-ui";
import axios from "axios";

export default {
  name: "ModelSetupModalComponent",
  components: {
    NModal, NCard, NH5, NUpload, NButton, NSpace
  },
  setup() {
    const defaultURL = inject('defaultURL')

    const show = ref(false)
    const wordsCount = ref(0)
    const fileLoading = ref(false)

    async function showModal() {
      this.show = true
      await countWords()
    }

    async function countWords() {
      try {
        const res = await axios.get(`${defaultURL}/api/countWords`)
        wordsCount.value = res.data
      } catch (e) {
        console.error(e)
      } finally {
      }
    }

    async function afterUpload() {
      await countWords()
      fileLoading.value = false
    }

    async function beforeUpload(e) {
      console.log(e)
      fileLoading.value = true
      return true
    }

    countWords().then()

    return {
      show, showModal, defaultURL, wordsCount, countWords, beforeUpload, fileLoading, afterUpload
    }
  }
}
</script>

<style scoped>

</style>