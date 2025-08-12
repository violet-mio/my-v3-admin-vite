<template>
  <span>{{ displayText }}</span>
</template>
<script setup>
import { ref, onMounted, computed } from "vue"

const value = ref([])
const options = ref([])

const props = defineProps({
  params: {
    type: Object
  }
})

const displayText = computed(() => {
  if (!value.value || value.value.length === 0) {
    return ""
  }
  if (!options.value || options.value.length === 0) {
    return value.value.join(", ")
  }

  const labelList = options.value.filter((item) => value.value.includes(item.value))
  return labelList.map((item) => item.label).join(", ")
})

const initValue = async () => {
  if (typeof props.params?.colDef?.getOptions === "function") {
    options.value = await props.params.colDef.getOptions()
  }
}

onMounted(() => {
  value.value = props.params.value
  initValue()
})

const refresh = (params) => {}

defineExpose({
  refresh
})
</script>

<style scoped />
