<template>
  <div class="visit-records-page">
    <transition name="fade" mode="out-in">
      <RecordList
        v-if="currentView === 'list'"
        :records="records"
        @view-detail="handleViewDetail"
      />
      <RecordDetail
        v-else
        :record="selectedRecord"
        @back="handleBack"
      />
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { visitRecords } from '../mock/records'
import RecordList from '../components/RecordList.vue'
import RecordDetail from '../components/RecordDetail.vue'

const records = ref(visitRecords)
const currentView = ref('list')
const selectedRecord = ref(null)

const handleViewDetail = (record) => {
  selectedRecord.value = record
  currentView.value = 'detail'
}

const handleBack = () => {
  currentView.value = 'list'
}
</script>

<style scoped>
.visit-records-page {
  min-height: 100vh;
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;
  box-sizing: border-box;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
