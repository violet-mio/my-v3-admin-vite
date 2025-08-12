<template>
  <template v-for="(item, index) of value" :key="item">
    <span>{{ labelMap[item] || item }}<span v-if="index < value.length - 1">,</span></span>
  </template>
</template>

<script setup>
import { ref, onMounted } from "vue"

const value = ref([])
const labelMap = ref([])

const props = defineProps({
  params: {
    type: Object
  }
})

onMounted(() => {
  value.value = props.params.value
  labelMap.value = props.params.colDef.refData || {}
})

const refresh = (params) => {}

defineExpose({
  refresh
})
</script>

<style scoped></style>
