<template>
  <n-button @click="modelSetupRef.showModal()">Модель</n-button>

  <ModelSetupModalComponent ref="modelSetupRef"></ModelSetupModalComponent>
</template>

<script>
import {NCard, NH5, NText, NUpload, NSpace, NButton, NInput} from "naive-ui";
import ModelSetupModalComponent from "@/components/ModelSetupModalComponent";
import {ref} from "vue";

export default {
  name: 'HelloWorld',
  components: {
    ModelSetupModalComponent,
    NCard, NH5, NText, NUpload, NSpace, NButton, NInput
  },
  setup() {
    const modelSetupRef = ref(null)

    return {
      modelSetupRef
    }
  }
}
</script>
