<template>
  <NLayoutHeader
    bordered
    position="absolute"
    class="h-16 shadow flex items-center z-10 pr-4 bg-blue-gray-100"
  >
    <div class="self-stretch overflow-x-auto">
      <div class="h-full flex items-center w-max-content">
        <RouterLink
          to="/"
          class="self-stretch flex items-center px-4 border-r hover:bg-light-900 duration-300"
        >
          <img src="@/assets/hpcgame_logo.svg" class="h-12 w-auto" />
          <div class="pl-4 text-2xl whitespace-nowrap">HPC Game</div>
        </RouterLink>
        <NMenu mode="horizontal" :options="menuOptions" />
      </div>
    </div>
    <div class="flex-1 h-full border-r"></div>
    <RouterLink v-slot="{ navigate }" to="/messages" custom>
      <NButton @click="navigate" class="px-4" text> 通知 </NButton>
    </RouterLink>
    <UserIndicatorProxy />
  </NLayoutHeader>
</template>

<script setup lang="ts">
import { NLayoutHeader, NMenu, type MenuOption, NButton } from 'naive-ui'
import { h, computed } from 'vue'
import { RouterLink } from 'vue-router'
import UserIndicatorProxy from '@/components/user/UserIndicatorProxy.vue'
import { isAdmin, loggedIn } from '@/api'

const menuOptions = computed<MenuOption[]>(() =>
  (
    [
      ['problems', '题目', '/problems', loggedIn.value],
      ['ranklist', '排行榜', '/ranklist'],
      ['admin', '管理', '/admin', isAdmin.value]
    ] as const
  ).map(([key, label, to, show]) => ({
    key,
    label: () => h(RouterLink, { to }, () => label),
    show: show ?? true
  }))
)
</script>

<style>
.app-logo {
  @apply h-6;
}

.w-max-content {
  width: max-content;
}
</style>
