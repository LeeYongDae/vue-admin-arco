<template>
  <a-card class="general-card" :title="'monitor.studioStatus.title.studioStatus'">
    <template #extra>
      <a-tag color="green">{{ 'monitor.studioStatus.smooth' }}</a-tag>
    </template>
    <a-descriptions layout="horizontal" :data="dataStatus" :column="2">
      <template #label="{ label }">
        <span v-if="['mainstream', 'hotStandby', 'coldStandby'].includes(label)">
          <a-typography-text style="padding-right: 8px">
            {{ `monitor.studioStatus.${label}`) }}
          </a-typography-text>
          {{ 'monitor.studioStatus.bitRate' }}
        </span>
        <span v-else>{{ label }}</span>
      </template>
    </a-descriptions>
    <a-typography-title style="margin-bottom: 16px" :heading="6">
      {{ 'monitor.studioStatus.title.pictureInfo' }}
    </a-typography-title>
    <a-descriptions layout="horizontal" :data="dataPicture" :column="2" />
  </a-card>
</template>

<script lang="ts" setup>
import { computed } from 'vue';

const dataStatus = computed(() => [
  {
    label: 'mainstream',
    value: '6 Mbps',
  },
  {
    label: 'monitor.studioStatus.frameRate',
    value: '60',
  },
  {
    label: 'hotStandby',
    value: '6 Mbps',
  },
  {
    label: 'monitor.studioStatus.frameRate',
    value: '60',
  },
  {
    label: 'coldStandby',
    value: '6 Mbps',
  },
  {
    label: 'monitor.studioStatus.frameRate',
    value: '60',
  },
])
const dataPicture = computed(() => [
  {
    label: 'monitor.studioStatus.line',
    value: '热备',
  },
  {
    label: 'CDN',
    value: 'KS',
  },
  {
    label: 'monitor.studioStatus.play',
    value: 'FLV',
  },
  {
    label: 'monitor.studioStatus.pictureQuality',
    value: '原画',
  },
])
</script>

<style scoped lang="less">
:deep(.arco-descriptions-item-label) {
  padding-right: 6px;
}
</style>
