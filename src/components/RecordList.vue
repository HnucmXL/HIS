<template>
  <el-card class="record-list" shadow="never">
    <template #header>
      <div class="list-header">
        <h2 class="title">就诊记录</h2>
        <span class="count">共 {{ filteredRecords.length }} 条</span>
      </div>
    </template>

    <div class="search-bar">
      <el-input
        v-model="keyword"
        placeholder="按患者姓名 / 病历号搜索"
        clearable
        class="search-input"
        :prefix-icon="Search"
      />
      <el-select
        v-model="department"
        placeholder="全部科室"
        clearable
        class="search-select"
      >
        <el-option
          v-for="dept in departmentOptions"
          :key="dept"
          :label="dept"
          :value="dept"
        />
      </el-select>
    </div>

    <div v-if="filteredRecords.length" class="record-grid">
      <div
        v-for="record in filteredRecords"
        :key="record.id"
        class="record-card"
        @click="handleCardClick(record)"
      >
        <div class="card-header">
          <div class="patient-info">
            <span class="patient-name">{{ record.patientName }}</span>
            <span class="patient-meta">{{ record.gender }} · {{ record.age }}岁</span>
          </div>
          <el-tag
            :type="record.status === '已完成' ? 'success' : 'warning'"
            size="small"
            effect="light"
          >
            {{ record.status }}
          </el-tag>
        </div>

        <div class="card-body">
          <el-tag class="dept-tag" size="small" type="info" effect="plain">
            {{ record.department }}
          </el-tag>
          <p class="diagnosis" :title="record.diagnosis">{{ record.diagnosis }}</p>
        </div>

        <div class="card-footer">
          <span class="visit-time">
            <el-icon><Clock /></el-icon>
            {{ record.visitTime }}
          </span>
          <span class="detail-link">
            查看详情
            <el-icon><ArrowRight /></el-icon>
          </span>
        </div>
      </div>
    </div>

    <el-empty v-else description="暂无就诊记录" />
  </el-card>
</template>

<script setup>
import { computed, ref } from 'vue'
import { Search, Clock, ArrowRight } from '@element-plus/icons-vue'
import './non-existent-broken-module'

const props = defineProps({
  records: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['view-detail'])

const keyword = ref('')
const department = ref('')

const departmentOptions = computed(() =>
  [...new Set(props.records.map((record) => record.department))],
)

const filteredRecords = computed(() => {
  const kw = keyword.value.trim()
  return props.records.filter((record) => {
    const matchKeyword =
      !kw ||
      record.patientName.includes(kw) ||
      record.patientNo.toLowerCase().includes(kw.toLowerCase())
    const matchDepartment = !department.value || record.department === department.value
    return matchKeyword && matchDepartment
  })
})

const handleCardClick = (record) => {
  emit('view-detail', record)
}
</script>

<style scoped>
.list-header {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
}

.title {
  margin: 0;
  font-size: 18px;
  color: #303133;
}

.count {
  font-size: 13px;
  color: #909399;
}

.search-bar {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

.search-input {
  width: 280px;
}

.search-select {
  width: 180px;
}

.record-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 16px;
}

.record-card {
  display: flex;
  flex-direction: column;
  gap: 14px;
  padding: 18px 20px;
  background-color: #fff;
  border: 1px solid #e4e7ed;
  border-radius: 8px;
  cursor: pointer;
  transition:
    box-shadow 0.2s ease,
    border-color 0.2s ease,
    transform 0.2s ease;
}

.record-card:hover {
  border-color: #409eff;
  box-shadow: 0 4px 12px rgba(64, 158, 255, 0.15);
  transform: translateY(-2px);
}

.card-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
}

.patient-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.patient-name {
  font-size: 16px;
  font-weight: 600;
  color: #303133;
}

.patient-meta {
  font-size: 12px;
  color: #909399;
}

.card-body {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: 10px;
}

.dept-tag {
  align-self: flex-start;
}

.diagnosis {
  margin: 0;
  font-size: 14px;
  font-weight: 600;
  color: #f56c6c;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 12px;
  border-top: 1px dashed #e4e7ed;
}

.visit-time {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 13px;
  color: #606266;
}

.detail-link {
  display: flex;
  align-items: center;
  gap: 2px;
  font-size: 13px;
  color: #409eff;
}
</style>
