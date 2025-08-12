<template>
  <el-select ref="multiSelectRef" v-model="value" multiple size="small" style="width: 100%">
    <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
  </el-select>
</template>

<script setup>
import { onMounted, nextTick, ref } from "vue"
// 参考：https://xubiaosunny.top/post/ag-grid_custom_cell_editor_te62.html

const props = defineProps({
  params: {
    type: Object,
    required: true
  }
})

const multiSelectRef = ref(null)
const value = ref([])
const options = ref([])

const getValue = () => {
  return value.value
}

const initValue = () => {
  value.value = props.params.value
  const labelMap = props.params.colDef.refData || {}
  options.value = props.params.values.map((item) => {
    return {
      label: labelMap[item] || item,
      value: item
    }
  })
}

onMounted(() => {
  initValue()
  nextTick(() => {
    multiSelectRef.value.focus()
  })
})

defineExpose({
  getValue
})
</script>

<style scoped></style>
