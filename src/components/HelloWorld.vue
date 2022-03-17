<template>
  <n-space vertical align="center">
    <n-grid :cols="12" style="width: 80%">
      <n-grid-item :span="12">
        <img src="ngram.png" alt="logo"/>
      </n-grid-item>

      <n-grid-item :span="12">
        <div class="items-center">
          <n-auto-complete
              placeholder="Введите запрос"
              :options="autoCompleteOptions"
              :loading="autoCompleteLoading"
              :on-update:value="handleAutocompleteUpdate"
              :render-label="renderLabel"
          />
        </div>
      </n-grid-item>

      <n-grid-item class="mt-2" :span="12">
        <div class="items-center">
          <Spellcheck v-model:text="textRef"/>
        </div>
      </n-grid-item>

      <n-grid-item class="mt-2" :span="12">
        <div class="items-center">
          <n-button @click="modelSetupRef.showModal()">Модель</n-button>
        </div>
      </n-grid-item>
    </n-grid>

  </n-space>

  <ModelSetupModalComponent ref="modelSetupRef"></ModelSetupModalComponent>
</template>

<script>
import {NCard, NH5, NText, NUpload, NSpace, NButton, NInput, NGrid, NGridItem, NAutoComplete, NTag} from "naive-ui";
import ModelSetupModalComponent from "@/components/ModelSetupModalComponent";
import {h, inject, ref} from "vue";
import axios from "axios";
import Spellcheck from "@/components/Spellcheck";

export default {
  name: 'HelloWorld',
  components: {
    Spellcheck,
    ModelSetupModalComponent,
    NCard, NH5, NText, NUpload, NSpace, NButton, NInput, NGrid, NGridItem, NAutoComplete
  },
  setup() {
    const defaultURL = inject('defaultURL')

    const textRef = ref("")
    const modelSetupRef = ref(null)

    const autoCompleteOptions = ref([])
    const autoCompleteLoading = ref(false)

    async function handleAutocompleteUpdate(text) {
      textRef.value = text
      if (!text) {
        autoCompleteOptions.value = []
        return
      }
      autoCompleteLoading.value = true

      try {
        const res = await axios.get(`${defaultURL}/api/getAutocomplete`, {params: {
            start_string: text.toLowerCase()
          }})
        autoCompleteOptions.value = res.data
        console.log(res.data)
      } finally {
        autoCompleteLoading.value = false
      }
    }

    function renderLabel(option) {
      return [
        option[0].join(' '),
        " ",
        h(NTag, { size: "small", type: "info" }, { default: () => option[1].toString().substr(0, 6) })
      ]

    }

    return {
      modelSetupRef, autoCompleteOptions, autoCompleteLoading, handleAutocompleteUpdate, renderLabel, textRef
    }
  }
}
</script>
