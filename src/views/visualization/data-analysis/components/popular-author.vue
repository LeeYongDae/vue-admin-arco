<template>
  <a-spin :loading="loading" style="width: 100%">
    <a-card class="general-card" :header-style="{ paddingBottom: '14px' }">
      <template #title>
        {{ 'dataAnalysis.popularAuthor' }}
      </template>
      <template #extra>
        <a-link>{{ 'workplace.viewMore' }}</a-link>
      </template>
      <a-table :data="tableData.list" :pagination="false" :bordered="false" style="margin-bottom: 20px" :scroll="{ x: '100%', y: '350px' }">
        <template #columns>
          <a-table-column :title="'dataAnalysis.popularAuthor.column.ranking'" data-index="ranking"></a-table-column>
          <a-table-column :title="'dataAnalysis.popularAuthor.column.author'" data-index="author"></a-table-column>
          <a-table-column
            :title="'dataAnalysis.popularAuthor.column.content'"
            data-index="contentCount"
            :sortable="{
              sortDirections: ['ascend', 'descend'],
            }"
          ></a-table-column>
          <a-table-column
            :title="'dataAnalysis.popularAuthor.column.click'"
            data-index="clickCount"
            :sortable="{
              sortDirections: ['ascend', 'descend'],
            }"
          ></a-table-column>
        </template>
      </a-table>
    </a-card>
  </a-spin>
</template>

<script lang="ts" setup>
import { PopularAuthorRes, queryPopularAuthor } from '@/api/visualization';
import useLoading from '@/hooks/loading';
import { ref } from 'vue';

const { loading, setLoading } = useLoading()
const tableData = ref<PopularAuthorRes>({ list: [] })
const fetchData = async () => {
  try {
    setLoading(true)
    const { data } = await queryPopularAuthor()
    tableData.value = data
  } catch (err) {
    // you can report use errorHandler or other
  } finally {
    setLoading(false)
  }
}
fetchData()
</script>

<style scoped lang="less">
.general-card {
  max-height: 425px;
}
</style>
