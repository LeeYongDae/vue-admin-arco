<template>
  <div class="navbar">
    <div class="left-side">
      <a-space>
        <icon-menu-fold
          v-if="!topMenu && appStore.device === 'mobile'"
          style="font-size: 22px; cursor: pointer"
          @click="toggleDrawerMenu"
        />
      </a-space>
    </div>
    <ul class="right-side">
      <li>
        <a-tooltip :content="theme === 'light' ? 'settings.navbar.theme.toDark' : 'settings.navbar.theme.toLight'">
          <a-button class="nav-btn" type="outline" :shape="'circle'" @click="handleToggleTheme">
            <template #icon>
              <icon-moon-fill v-if="theme === 'dark'" />
              <icon-sun-fill v-else />
            </template>
          </a-button>
        </a-tooltip>
      </li>
      <li>
        <a-tooltip :content="'settings.navbar.alerts'">
          <div class="message-box-trigger">
            <a-badge :count="9" dot>
              <a-button class="nav-btn" type="outline" :shape="'circle'" @click="setPopoverVisible">
                <icon-notification />
              </a-button>
            </a-badge>
          </div>
        </a-tooltip>
        <a-popover
          trigger="click"
          :arrow-style="{ display: 'none' }"
          :content-style="{ padding: 0, minWidth: '400px' }"
          content-class="message-popover"
        >
          <div ref="refBtn" class="ref-btn"></div>
          <template #content>
            <message-box />
          </template>
        </a-popover>
      </li>
      <li>
        <a-tooltip :content="isFullscreen ? 'settings.navbar.screen.toExit' : 'settings.navbar.screen.toFull'">
          <a-button class="nav-btn" type="outline" :shape="'circle'" @click="toggleFullScreen">
            <template #icon>
              <icon-fullscreen-exit v-if="isFullscreen" />
              <icon-fullscreen v-else />
            </template>
          </a-button>
        </a-tooltip>
      </li>
      <li>
        <a-tooltip :content="'settings.title'">
          <a-button class="nav-btn" type="outline" :shape="'circle'" @click="setVisible">
            <template #icon>
              <icon-settings />
            </template>
          </a-button>
        </a-tooltip>
      </li>
      <li>
        <a-dropdown trigger="click">
          <a-avatar :size="32" :style="{ marginRight: '8px' }">
            <img alt="avatar" :src="avatar" />
          </a-avatar>
          <template #content>
            <a-doption>
              <a-space @click="switchGit">
                <icon-github />
                <span>开源地址</span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="switchRoles">
                <icon-tag />
                <span>
                  {{ 'messageBox.switchRoles' }}
                </span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="$router.push({ name: 'info' })">
                <icon-user />
                <span>
                  {{ 'messageBox.userCenter' }}
                </span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="$router.push({ name: 'setting' })">
                <icon-settings />
                <span>
                  {{ 'messageBox.userSettings' }}
                </span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="open('/shop-vite')">
                <icon-tag />
                <span>shop vite 付费版本</span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="open('/admin-plus')">
                <icon-tag />
                <span>admin plus 付费版本</span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="open('/admin-pro')">
                <icon-tag />
                <span>admin pro 付费版本</span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="open('/vue-admin-better')">
                <icon-tag />
                <span>admin better 开源版</span>
              </a-space>
            </a-doption>
            <a-doption>
              <a-space @click="handleLogout">
                <icon-export />
                <span>
                  {{ 'messageBox.logout' }}
                </span>
              </a-space>
            </a-doption>
          </template>
        </a-dropdown>
      </li>
    </ul>
  </div>
</template>

<script lang="ts" setup>
import useUser from '@/hooks/user'
import { useAppStore, useUserStore } from '@/store'
import { Message } from '@arco-design/web-vue'
import { useDark, useFullscreen, useToggle } from '@vueuse/core'
import { computed, inject, ref } from 'vue'
import MessageBox from '../message-box/index.vue'

const appStore = useAppStore()
const userStore = useUserStore()
const { logout } = useUser()

const { isFullscreen, toggle: toggleFullScreen } = useFullscreen()
const avatar = computed(() => {
  return userStore.avatar
})
const theme = computed(() => {
  return appStore.theme
})
const topMenu = computed(() => appStore.topMenu && appStore.menu)
const isDark = useDark({
  selector: 'body',
  attribute: 'arco-theme',
  valueDark: 'dark',
  valueLight: 'light',
  storageKey: 'arco-theme',
  onChanged(dark: boolean) {
    // overridden default behavior
    appStore.toggleTheme(dark)
  },
})
const toggleTheme = useToggle(isDark)
const handleToggleTheme = () => {
  toggleTheme()
}
const setVisible = () => {
  appStore.updateSettings({ globalSettings: true })
}
const refBtn = ref()

const setPopoverVisible = () => {
  const event = new MouseEvent('click', {
    view: window,
    bubbles: true,
    cancelable: true,
  })
  refBtn.value.dispatchEvent(event)
}
const handleLogout = () => {
  logout()
}

const switchRoles = async () => {
  const res = await userStore.switchRoles()
  Message.success(res as string)
}
const toggleDrawerMenu = inject('toggleDrawerMenu') as () => void
const switchGit = () => {
  window.open('https://github.com/zxwk1998/vue-admin-arco')
}
const open = (val: string) => {
  window.open(`https://vuejs-core.cn/${val}`)
}
</script>

<style scoped lang="less">
.navbar {
  display: flex;
  justify-content: space-between;
  height: 100%;
  background-color: var(--color-bg-2);
  border-bottom: 1px solid var(--color-border);
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  height: 60px;
  z-index: 99;
}

.left-side {
  display: flex;
  align-items: center;
  padding-left: 20px;
}

.right-side {
  display: flex;
  padding-right: 20px;
  list-style: none;

  :deep(.locale-select) {
    border-radius: 20px;
  }

  li {
    display: flex;
    align-items: center;
    padding: 0 10px;
  }

  a {
    color: var(--color-text-1);
    text-decoration: none;
  }

  .nav-btn {
    border-color: rgb(var(--gray-2));
    color: rgb(var(--gray-8));
    font-size: 16px;
  }

  .trigger-btn,
  .ref-btn {
    position: absolute;
    bottom: 14px;
  }

  .trigger-btn {
    margin-left: 14px;
  }
}
</style>

<style lang="less">
.message-popover {
  .arco-popover-content {
    margin-top: 0;
  }
}

.arco-dropdown-list-wrapper {
  max-height: 100vh !important;
}
</style>
