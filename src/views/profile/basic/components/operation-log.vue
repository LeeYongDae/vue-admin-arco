<template>
  <a-card class="general-card">
    <template #title>
      {{ 'basicProfile.title.operationLog' }}
    </template>
    <a-spin :loading="loading" style="width: 100%">
      <a-table :data="renderData">
        <template #columns>
          <a-table-column :title="'basicProfile.column.contentNumber'" data-index="contentNumber" />
          <a-table-column :title="'basicProfile.column.updateContent'" data-index="updateContent" />
          <a-table-column :title="'basicProfile.column.status'" data-index="status">
            <template #cell="{ record }">
              <p v-if="record.status === 0">
                <span class="circle"></span>
                <span>{{ 'basicProfile.cell.auditing' }}</span>
              </p>
              <p v-if="record.status === 1">
                <span class="circle pass"></span>
                <span>{{ 'basicProfile.cell.pass' }}</span>
              </p>
            </template>
          </a-table-column>
          <a-table-column :title="'basicProfile.column.updateTime'" data-index="updateTime" />
          <a-table-column :title="'basicProfile.column.operation'">
            <template #cell>
              <a-button type="text">{{ 'basicProfile.cell.view' }}</a-button>
            </template>
          </a-table-column>
        </template>
      </a-table>
    </a-spin>
  </a-card>
</template>

<script lang="ts" setup>
import { operationLogRes, queryOperationLog } from '@/api/profile';
import useLoading from '@/hooks/loading';
import { ref } from 'vue';

const { loading, setLoading } = useLoading(true)
const renderData = ref<operationLogRes>([])
const fetchData = async () => {
  try {
    const { data } = await queryOperationLog()
    renderData.value = data
  } catch (err) {
    // you can report use errorHandler or other
  } finally {
    setLoading(false)
  }
}
fetchData()
</script>

<style scoped lang="less">
:deep(.arco-table-th) {
  &:last-child {
    .arco-table-th-item-title {
      margin-left: 16px;
    }
  }
}
</style>
