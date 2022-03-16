<template>
  <n-button :loading="loading" @click="spellcheck">Spellcheck</n-button>
  <n-collapse-transition :show="showResults">
    {{results}}
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
  NInput,
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
    NCard, NH5, NText, NUpload, NSpace, NButton, NInput, NGrid, NGridItem, NAutoComplete, NCollapseTransition
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