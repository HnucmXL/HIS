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

    <el-table
      :data="pagedRecords"
      stripe
      row-key="id"
      style="width: 100%"
      class="record-table"
      @row-click="handleRowClick"
    >
      <el-table-column prop="patientName" label="患者姓名" min-width="120">
        <template #default="{ row }">
          <div class="patient-cell">
            <span class="patient-name">{{ row.patientName }}</span>
            <span class="patient-meta">{{ row.gender }} · {{ row.age }}岁</span>
          </div>
        </template>
      </el-table-column>
      <el-table-column prop="department" label="科室" min-width="120" />
      <el-table-column prop="diagnosis" label="诊断" min-width="200" show-overflow-tooltip />
      <el-table-column prop="visitTime" label="就诊时间" min-width="160" />
      <el-table-column label="操作" width="100" fixed="right" align="center">
        <template #default>
          <el-button link type="primary">查看详情</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-empty v-if="!pagedRecords.length" description="暂无就诊记录" />

    <div v-if="filteredRecords.length" class="pagination-wrap">
      <el-pagination
        v-model:current-page="currentPage"
        :page-size="pageSize"
        :total="filteredRecords.length"
        layout="total, prev, pager, next"
        background
      />
    </div>
  </el-card>
</template>

<script setup>
import { computed, ref, watch } from 'vue'
import { Search } from '@element-plus/icons-vue'

const props = defineProps({
  records: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['view-detail'])

const keyword = ref('')
const department = ref('')
const currentPage = ref(1)
const pageSize = 5

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

const pagedRecords = computed(() => {
  const start = (currentPage.value - 1) * pageSize
  return filteredRecords.value.slice(start, start + pageSize)
})

watch([keyword, department], () => {
  currentPage.value = 1
})

const handleRowClick = (row) => {
  emit('view-detail', row)
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
  margin-bottom: 16px;
}

.search-input {
  width: 280px;
}

.search-select {
  width: 180px;
}

.record-table {
  cursor: pointer;
}

.patient-cell {
  display: flex;
  flex-direction: column;
}

.patient-name {
  color: #303133;
}

.patient-meta {
  font-size: 12px;
  color: #909399;
}

.pagination-wrap {
  display: flex;
  justify-content: flex-end;
  margin-top: 16px;
}
</style>
