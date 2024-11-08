<template>
  <div class="container">
    <Breadcrumb :items="['menu.profile', 'menu.profile.basic']" />
    <a-space direction="vertical" :size="16" fill>
      <a-card class="general-card" :title="'basicProfile.title.form'">
        <template #extra>
          <a-space>
            <a-button>{{ 'basicProfile.cancel' }}</a-button>
            <a-button type="primary">
              {{ 'basicProfile.goBack' }}
            </a-button>
          </a-space>
        </template>
        <a-steps v-model:current="step" line-less class="steps">
          <a-step>{{ 'basicProfile.steps.commit' }}</a-step>
          <a-step>{{ 'basicProfile.steps.approval' }}</a-step>
          <a-step>{{ 'basicProfile.steps.finish' }}</a-step>
        </a-steps>
      </a-card>
      <a-card class="general-card">
        <ProfileItem :loading="loading" :render-data="currentData" />
      </a-card>
      <a-card class="general-card">
        <ProfileItem :loading="preLoading" type="pre" :render-data="preData" />
      </a-card>
      <OperationLog />
    </a-space>
  </div>
</template>

<script lang="ts" setup>
import { ProfileBasicRes, queryProfileBasic } from '@/api/profile'
import useLoading from '@/hooks/loading'
import { ref } from 'vue'
import OperationLog from './components/operation-log.vue'
import ProfileItem from './components/profile-item.vue'

const { loading, setLoading } = useLoading(true)
const { loading: preLoading, setLoading: preSetLoading } = useLoading(true)
const currentData = ref<ProfileBasicRes>({} as ProfileBasicRes)
const preData = ref<ProfileBasicRes>({} as ProfileBasicRes)
const step = ref(1)
const fetchCurrentData = async () => {
  try {
    const { data } = await queryProfileBasic()
    currentData.value = data
    step.value = 2
  } catch (err) {
    // you can report use errorHandler or other
  } finally {
    setLoading(false)
  }
}
const fetchPreData = async () => {
  try {
    const { data } = await queryProfileBasic()
    preData.value = data
  } catch (err) {
    // you can report use errorHandler or other
  } finally {
    preSetLoading(false)
  }
}
fetchCurrentData()
fetchPreData()
</script>

<script lang="ts">
export default {
  name: 'Basic',
}
</script>

<style scoped lang="less">
.container {
  padding: 0 20px 20px 20px;
}

.steps {
  max-width: 548px;
  margin: 0 auto 10px;
}
</style>
