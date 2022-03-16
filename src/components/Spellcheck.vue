<template>
  <n-button :loading="loading" @click="spellcheck">Spellcheck</n-button>
  <n-collapse-transition :show="showResults">
    <n-space vertical>
      <n-p v-html="result[0].join(' ')" v-for="result in results"></n-p>
    </n-space>
  </n-collapse-transition>
</template>

<script>
import {
  NAutoComplete,
  NButton,
  NCard,
  NCollapseTransition,
  NGrid,
  NGridItem,
  NH5,
  NInput, NList, NP,
  NSpace,
  NText,
  NUpload
} from "naive-ui";
import {inject, ref} from "vue";
import axios from "axios";

export default {
  name: "Spellcheck",
  props: ['text'],
  components: {
    NCard, NH5, NP, NText, NUpload, NSpace, NButton, NInput, NGrid, NGridItem, NAutoComplete, NCollapseTransition,
    NList
  },
  setup(props) {
    const defaultURL = inject('defaultURL')

    const loading = ref(false)
    const results = ref([])
    const showResults = ref(false)

    async function spellcheck() {
      loading.value = true
      try {
        const res = await axios.get(`${defaultURL}/api/spellcheck`, {params: {
            q: props.text.toLowerCase()
          }})
        results.value = res.data
        showResults.value = true
      } finally {
        loading.value = false
      }
    }
    return {
      loading, results, showResults, spellcheck
    }
  }
}
</script>

<style scoped>

</style>