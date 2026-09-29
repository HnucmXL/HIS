<template>
  <el-card class="record-detail" shadow="never">
    <template #header>
      <div class="detail-header">
        <el-button :icon="ArrowLeft" @click="emit('back')">返回列表</el-button>
        <h2 class="title">就诊详情</h2>
      </div>
    </template>

    <template v-if="record">
      <el-descriptions title="患者基本信息" :column="2" border class="section">
        <el-descriptions-item label="姓名">{{ record.patientName }}</el-descriptions-item>
        <el-descriptions-item label="性别">{{ record.gender }}</el-descriptions-item>
        <el-descriptions-item label="年龄">{{ record.age }} 岁</el-descriptions-item>
        <el-descriptions-item label="病历号">{{ record.patientNo }}</el-descriptions-item>
        <el-descriptions-item label="联系电话" :span="2">{{ record.phone }}</el-descriptions-item>
      </el-descriptions>

      <el-descriptions title="就诊信息" :column="2" border class="section">
        <el-descriptions-item label="就诊号">{{ record.visitNo }}</el-descriptions-item>
        <el-descriptions-item label="就诊时间">{{ record.visitTime }}</el-descriptions-item>
        <el-descriptions-item label="科室">{{ record.department }}</el-descriptions-item>
        <el-descriptions-item label="接诊医生">{{ record.doctor }}</el-descriptions-item>
        <el-descriptions-item label="就诊类型">{{ record.visitType }}</el-descriptions-item>
        <el-descriptions-item label="状态">
          <el-tag :type="record.status === '已完成' ? 'success' : 'warning'" size="small">
            {{ record.status }}
          </el-tag>
        </el-descriptions-item>
        <el-descriptions-item label="诊断" :span="2">
          <span class="diagnosis">{{ record.diagnosis }}</span>
        </el-descriptions-item>
        <el-descriptions-item label="费用" :span="2">
          <span class="cost">¥ {{ record.cost.toFixed(2) }}</span>
        </el-descriptions-item>
      </el-descriptions>

      <div class="section">
        <h3 class="section-title">病历内容</h3>
        <div class="medical-record">
          <div class="record-item">
            <span class="record-label">主诉</span>
            <p class="record-content">{{ record.chiefComplaint }}</p>
          </div>
          <div class="record-item">
            <span class="record-label">现病史</span>
            <p class="record-content">{{ record.presentIllness }}</p>
          </div>
          <div class="record-item">
            <span class="record-label">既往史</span>
            <p class="record-content">{{ record.pastHistory }}</p>
          </div>
          <div class="record-item">
            <span class="record-label">体格检查</span>
            <p class="record-content">{{ record.examination }}</p>
          </div>
        </div>
      </div>

      <div class="section">
        <h3 class="section-title">医嘱处方</h3>
        <el-table :data="record.prescription" border style="width: 100%">
          <el-table-column type="index" label="序号" width="70" align="center" />
          <el-table-column prop="name" label="药品名称" min-width="180" />
          <el-table-column prop="specification" label="规格" min-width="130" />
          <el-table-column prop="usage" label="用法" min-width="90" align="center" />
          <el-table-column prop="amount" label="剂量" min-width="200" />
        </el-table>
      </div>

      <div class="section">
        <h3 class="section-title">医生建议</h3>
        <el-alert type="info" :closable="false" show-icon :title="record.advice" />
      </div>
    </template>

    <el-empty v-else description="未找到就诊记录" />
  </el-card>
</template>

<script setup>
import { ArrowLeft } from '@element-plus/icons-vue'

defineProps({
  record: {
    type: Object,
    default: null,
  },
})

const emit = defineEmits(['back'])
</script>

<style scoped>
.detail-header {
  display: flex;
  align-items: center;
  gap: 16px;
}

.title {
  margin: 0;
  font-size: 18px;
  color: #303133;
}

.section {
  margin-bottom: 24px;
}

.section:last-child {
  margin-bottom: 0;
}

.section-title {
  margin: 0 0 12px;
  padding-left: 8px;
  border-left: 3px solid #409eff;
  font-size: 15px;
  color: #303133;
}

.diagnosis {
  font-weight: 600;
  color: #f56c6c;
}

.cost {
  font-weight: 600;
  color: #e6a23c;
}

.medical-record {
  border: 1px solid #e4e7ed;
  border-radius: 4px;
  padding: 16px 20px;
  background-color: #fafafa;
}

.record-item {
  display: flex;
  margin-bottom: 14px;
}

.record-item:last-child {
  margin-bottom: 0;
}

.record-label {
  flex-shrink: 0;
  width: 72px;
  font-weight: 600;
  color: #606266;
}

.record-content {
  margin: 0;
  color: #303133;
  line-height: 1.7;
}
</style>
