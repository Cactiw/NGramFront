<template>
  <n-modal style="max-width: 40%" class="bg-white" preset="card" title="Параметры модели" v-model:show="show">
    <n-space vertical align="center">
      <n-h5>Всего слов: {{wordsCount}}</n-h5>
      <n-space>
        <n-switch v-model:value="useSmooth"></n-switch>
        <n-p>Сглаживание</n-p>
      </n-space>
      <n-upload
          :action="defaultURL + `/api/addFile?&use_smooth=${useSmooth}`" @finish="afterUpload" @before-upload="beforeUpload" @error="fileLoading = false">
        <n-button ghost :loading="fileLoading" type="info">Добавить файл</n-button>
      </n-upload>
      <n-button ghost :loading="deleteLoading" type="error" @click="clearModel">Очистить модель</n-button>
    </n-space>
  </n-modal>
</template>

<script>
import {inject, ref} from "vue";
import {NModal, NCard, NH5, NUpload, NButton, NSpace, NCheckbox, NRadio, NSwitch, NP} from "naive-ui";
import axios from "axios";

export default {
  name: "ModelSetupModalComponent",
  components: {
    NModal, NCard, NH5, NUpload, NButton, NSpace, NCheckbox, NSwitch, NP
  },
  setup() {
    const defaultURL = inject('defaultURL')

    const show = ref(false)
    const wordsCount = ref(0)
    const fileLoading = ref(false)
    const deleteLoading = ref(false)

    const useSmooth = ref(false)

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

    async function clearModel() {
      console.log(useSmooth.value)
      deleteLoading.value = true
      try {
        const res = await axios.post(`${defaultURL}/api/clearModel`)
        await countWords()
      } finally {
        deleteLoading.value = false
      }
    }

    countWords().then()

    return {
      show, showModal, defaultURL, wordsCount, countWords, beforeUpload, fileLoading, afterUpload, deleteLoading,
      clearModel, useSmooth
    }
  }
}
</script>

<style scoped>

</style>