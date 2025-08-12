<template>
  <div>
    <ag-grid-vue
      class="ag-theme-alpine"
      :gridOptions="gridOptions"
      :rowData="rowData"
      :columnDefs="colDefs"
      @grid-ready="onGridReady"
      style="height: 500px"
    />
  </div>
</template>

<script setup>
import { computed, onMounted, ref, watch } from "vue"

import { AgGridVue } from "ag-grid-vue3"
import "ag-grid-community/styles/ag-grid.css"
import "ag-grid-community/styles/ag-theme-alpine.css"
import agAntdMultiSelectCellEditor from "./agAntdMultiSelectCellEditor.vue"
import agAntdMultiSelectRenderer from "./agAntdMultiSelectRenderer.vue"

const rowData = ref([
  { make: "Tesla", model: "Model Y", price: 64950, electric: true, user_id: [1, 2] },
  { make: "Ford", model: "F-Series", price: 33850, electric: false, user_id: [2] },
  { make: "Toyota", model: "Corolla", price: 29600, electric: false, user_id: [1, 2, 3] },
  { make: "Honda", model: "Civic", price: 22350, electric: false, user_id: [3] },
  { make: "BMW", model: "3 Series", price: 41450, electric: false, user_id: [1, 3] }
])
const gridApi = ref(null)
const onGridReady = (params) => {
  gridApi.value = params.api
}

const gridOptions = {
  defaultColDef: {
    flex: 1,
    minWidth: 100,
    resizable: true,
    sortable: true,
    filter: true,
    editable: true
  },
  // 全行编辑
  editType: "fullRow",
  // 单机行编辑
  singleClickEdit: true,
  // 禁止右键菜单
  suppressContextMenu: true
  // components: {
  //   agAntdMultiSelectCellEditor,
  //   agAntdMultiSelectRenderer
  // }
}

const optionsList = ref([])
onMounted(() => {
  // 使用promise和定时器mock请求
  setTimeout(() => {
    optionsList.value = [
      { label: "张三", value: 1 },
      { label: "李四", value: 2 },
      { label: "王五", value: 3 }
    ]

    gridApi.value?.setGridOption("columnDefs", colDefs.value)
    gridApi.value?.setGridOption("rowData", rowData.value)
    // gridApi.value?.refreshCells({ force: true })
  }, 3000)
})

// 提供一个获取选项的方法
const getOptions = () => {
  return new Promise((resolve) => {
    if (optionsList.value.length > 0) {
      resolve(optionsList.value)
    } else {
      // 监听数据变化
      const unwatch = watch(optionsList, (newVal) => {
        if (newVal.length > 0) {
          resolve(optionsList.value)
          unwatch() // 数据获取后取消监听
        }
      })
    }
  })
}

const colDefs = computed(() => {
  return [
    {
      headerName: "汽车品牌",
      field: "make",
      width: 150
    },
    // 使用自定义渲染器和编辑器
    {
      headerName: "用户",
      field: "user_id",
      editable: true,
      cellRenderer: "agAntdMultiSelectRenderer",
      cellEditor: "agAntdMultiSelectCellEditor",
      cellEditorParams: {
        // values: [1, 2, 3]
      },
      optionsList: optionsList.value,
      getOptions,
      width: 150
    }
  ]
})

defineExpose({
  agAntdMultiSelectRenderer,
  agAntdMultiSelectCellEditor
})
</script>

<style lang="scss" scoped></style>
