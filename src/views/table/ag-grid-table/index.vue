<template>
  <div>
    <ag-grid-vue
      class="ag-theme-alpine"
      :gridOptions="gridOptions"
      :rowData="rowData"
      :columnDefs="colDefs"
      style="height: 500px"
    />
  </div>
</template>

<script setup>
import { ref } from "vue"

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
}

// Column Definitions: Defines the columns to be displayed.
const colDefs = ref([
  {
    headerName: "汽车品牌",
    field: "make",
    width: 150
  },
  {
    headerName: "汽车型号",
    field: "model",
    width: 150,
    cellEditor: "agTextCellEditor"
  },
  {
    headerName: "价格",
    field: "price",
    width: 120,
    valueFormatter: (params) => `$${params.value.toLocaleString()}`
  },
  {
    headerName: "电动汽车",
    field: "electric",
    cellRenderer: (params) => (params.value ? "是" : "否"),
    width: 120
  },
  // 使用自定义渲染器和编辑器
  {
    headerName: "用户",
    field: "user_id",
    editable: true,
    cellRenderer: agAntdMultiSelectRenderer,
    cellEditor: agAntdMultiSelectCellEditor,
    cellEditorParams: {
      values: [1, 2, 3]
    },
    refData: {
      1: "张三",
      2: "李四",
      3: "王五"
    },
    width: 150
  }
  // 省略其他列配置
])
</script>

<style lang="scss" scoped></style>
