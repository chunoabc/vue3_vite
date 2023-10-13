<template>
  <div>
    <el-container>
      <el-header>
        <div style="margin-top: 20px">
          <el-button :icon="Plus" @click="onAddShow">新增</el-button>
          <el-button :icon="Plus" @click="onAddShow2">新增2</el-button>
        </div>
      </el-header>
      <el-main>
        <el-table :data="tableData" stripe style="width: 100%">
          <el-table-column prop="date" label="最後日期" />
          <el-table-column prop="name" label="待辦項目" />
          <el-table-column prop="status" label="狀態">
            <template #default="scope">
              <span v-if="scope.row.status === true" style="color:green;">完成</span>
              <span v-else-if="moment(scope.row.date) < moment()" style="color:red;">逾時</span>
              <span v-else style="color:#777">待辦</span>
            </template>
          </el-table-column>
          <el-table-column prop="operate" label="操作">
            <template #default="scope">
              <el-button link type="primary" size="small" @click="onChangeStatus(scope.$index)">修改狀態</el-button>
              <el-button link type="primary" size="small" @click="onDelete(scope.$index)">刪除</el-button>
            </template>
          </el-table-column>
        </el-table>

        <el-divider/>

        <table id="customers">
          <tr>
            <th>最後日期</th>
            <th>待辦項目</th>
            <th>狀態</th>
            <th>操作</th>
          </tr>
          <tr v-for="(item, index) in tableData" :key="index">
            <td>{{item.date}}</td>
            <td>{{item.name}}</td>
            <td>
              <span v-if="item.status === true" style="color:green;">完成</span>
              <span v-else-if="moment(item.date) < moment()" style="color:red;">逾時</span>
              <span v-else style="color:#777">待辦</span>
            </td>
            <td>
              <span class="span-operate" @click="onChangeStatus2(index)">修改</span>
              <span class="span-operate" @click="onDelete2(index)">刪除</span>
            </td>
          </tr>
        </table>

      </el-main>
    </el-container>
    <AddDia :show="addDiaShow" @close="onAddClose" @save="onSave"/>
    <AddDia2 ref="AddDia2Ref" @save="onSave2" />
  </div>
</template>
<script setup>
import { Plus } from '@element-plus/icons-vue'
import { ElMessageBox, ElMessage } from 'element-plus'
import { ref } from 'vue'
import moment from 'moment'
import AddDia from './components/AddDia.vue'
import AddDia2 from './components/AddDia2.vue'

const addDiaShow = ref(false)
const AddDia2Ref = ref()
const tableData = ref([
  {
    date: '2023-10-10',
    name: '爬山',
    status : false,
  },
  {
    date: '2023-10-22',
    name: '帶貓打疫苗',
    status : false,
  }
])

function onAddShow() {
  addDiaShow.value = true
}
function onAddClose() {
  addDiaShow.value = false
}

function onSave(data) {
  addDiaShow.value = false
  tableData.value.push({
    date : data.endDate,
    name : data.projectName,
    status : false
  })
}

function onChangeStatus(index) {
  ElMessageBox.confirm(
    '確定要修改狀態嗎?',
    '修改確認',
    {
      confirmButtonText: '確認',
      cancelButtonText: '取消',
    }
  ).then(() => {
    tableData.value[index].status = !tableData.value[index].status
    ElMessage({
      type: 'success',
      message: '修改成功!',
    })
  })
  .catch(() => {
  })
}

function onDelete(index) {
  ElMessageBox.confirm(
    '確定要刪除此項目嗎?',
    '刪除確認',
    {
      confirmButtonText: '確認',
      cancelButtonText: '取消',
    }
  ).then(() => {
    tableData.value.splice(index, 1)
    ElMessage({
      type: 'success',
      message: '刪除成功!',
    })
  })
  .catch(() => {
  })
}

function onAddShow2() {
  AddDia2Ref.value.show()
}

function onSave2(data) {
  tableData.value.push({
    date : data.endDate,
    name : data.projectName,
    status : false
  })
}

function onChangeStatus2(index) {
  if(confirm('確定要修改狀態嗎')) {
    alert('修改成功!')
    tableData.value[index].status = !tableData.value[index].status
  }
}

function onDelete2(index) {
  if(confirm('確定要刪除此項目嗎')) {
    alert('刪除成功!')
    tableData.value.splice(index, 1)
  }
}

</script>

<style scoped>
#customers {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#customers td, #customers th {
  text-align: center;
  vertical-align: middle;
  border: 1px solid #ddd;
  padding: 8px;
}

#customers tr:nth-child(even){background-color: #f2f2f2;}

#customers tr:hover {background-color: #ddd;}

#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: #777;
  color: white;
}

.span-operate {
  color: blue;
  margin-left: 10px;
  cursor: pointer;
}
</style>
