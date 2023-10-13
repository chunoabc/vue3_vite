<template>
  <div>
    <el-dialog v-model="dialogVisible" title="新增項目" width="40%" :close-on-click-modal="false" :show-close="false">
      <el-divider style="margin-top:-30px;"/>
      <el-row class="row-item">
        <el-col :span="24">截止時間</el-col>
        <el-col :span="24">
          <el-date-picker
            v-model="endDate"
            type="date"
            placeholder="截止時間"
            style="width: 100%"
            format="YYYY-MM-DD"
            value-format="YYYY-MM-DD"
          />
        </el-col>
      </el-row>
      <el-row class="row-item">
        <el-col :span="24">項目名稱</el-col>
        <el-col :span="24">
          <el-input v-model="projectName" placeholder="項目名稱" style="width: 100%;"/>
        </el-col>
      </el-row>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="onSave">
            新增
          </el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { ElMessage } from 'element-plus'
const emit = defineEmits( ['save'] )
const dialogVisible = ref(false)
const endDate = ref('')
const projectName = ref('')

function onSave() {
  if(!dataCheck()) {
    return
  }
  const data = {
    endDate : endDate.value,
    projectName : projectName.value
  }
  ElMessage({
    message: '新增成功!',
    type: 'success',
  })
  dialogVisible.value = false
  emit('save', data)
}

function dataCheck() {
  if(!endDate.value) {
    ElMessage({
      message: '請輸入截止日期.',
      type: 'error',
    })
    return false
  }
  if(!projectName.value) {
    ElMessage({
      message: '請輸入項目名稱.',
      type: 'error',
    })
    return false
  }

  return true
}

defineExpose( {
  show : function() {
    dialogVisible.value = true
    endDate.value = ''
    projectName.value = ''
  }
} )
</script>

<style scoped>
.row-item {
  margin-bottom: 10px;
}
</style>