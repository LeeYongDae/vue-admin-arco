<template>
  <div class="container">
    <Breadcrumb :items="['menu.result', 'menu.result.success']" />
    <div class="wrapper">
      <a-result class="result" status="success" :title="'success.result.title'" :subtitle="'success.result.subTitle'">
        <template #extra>
          <a-space class="operation-wrap" :size="16">
            <a-button key="again" type="secondary">
              {{ 'success.result.printResult' }}
            </a-button>
            <a-button key="back" type="primary">
              {{ 'success.result.projectList' }}
            </a-button>
          </a-space>
        </template>
      </a-result>

      <div class="steps-wrapper">
        <a-typography-paragraph bold>{{ 'success.result.progress' }}</a-typography-paragraph>
        <a-steps type="dot" :current="2">
          <a-step :title="'success.submitApplication'" description="2020/10/10 14:00:39" />
          <a-step :title="'success.leaderReview'" :description="'success.processing'" />
          <a-step :title="'success.purchaseCertificate'" :description="'success.waiting'" />
          <a-step :title="'success.safetyTest'" :description="'success.waiting'" />
          <a-step :title="'success.launched'" :description="'success.waiting'" />
        </a-steps>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup></script>

<script lang="ts">
export default {
  name: 'Success',
}
</script>

<style scoped lang="less">
.container {
  padding: 0 20px 20px 20px;
}
.wrapper {
  padding: 24px 150px;
  background-color: var(--color-bg-2);
  border-radius: 4px;
}

.result {
  margin: 150px 0 0 0;
}

.operation-wrap {
  margin-bottom: 40px;
  text-align: center;
}

.steps-wrapper {
  width: 100%;
  min-width: fit-content;
  margin-bottom: 150px;
  padding: 20px;
  background-color: rgb(var(--gray-1));
}
</style>

<style lang="less" scoped>
.mobile {
  .wrapper {
    padding: 24px 10px;
    .steps-wrapper {
      transform: scale(0.8);
    }
  }
}
</style>
