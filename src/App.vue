<script setup>
import axios from 'axios'
import Edit from './components/Edit.vue'
import axis from 'axios'
import { onMounted, ref } from 'vue'
// TODO: 列表渲染
let list = ref([])

const getList = async () => {
  const res = await axios.get('/list')
  list.value = res.data
  console.log(list)
}
// 在mounted或者created里面去调用
onMounted(() => {
  getList()
})

// TODO: 删除功能
let row = ref(0)
const deleteRow = async (id) => {
  console.log(id)
  axios.delete(`/del/${id}`)
  getList()
}

// TODO: 编辑功能
// 绑定dom
const edit = ref(null)
// 1.打开弹框   同时传入行号
const onEdit = (row) => {
  console.log('onEdit', edit)
  edit.value.dialogVisibleStatusChange(row)
 
}
// 2.回填数据（/调用详情接口）
// 3.更新数据
// const reLoad=()=>{
//    getList()
// }



</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }">
          <el-button type="primary" link @click="onEdit(row)">编辑</el-button>
          <el-button type="danger" link @click="deleteRow(row.id)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="edit" @on-update="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
