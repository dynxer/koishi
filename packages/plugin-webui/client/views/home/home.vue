<template>
  <div class="card-grid profile-grid">
    <k-numeric title="当前消息频率" icon="paper-plane">{{ currentRate }} / min</k-numeric>
    <k-numeric title="近期消息频率" icon="history" v-if="config.database">{{ recentRate }} / d</k-numeric>
    <k-numeric title="命名插件数量" icon="plug">{{ registry.pluginCount }}</k-numeric>
    <k-numeric title="数据库体积" icon="database" type="size" :value="meta.storageSize" v-if="meta" fallback="未安装"/>
    <k-numeric title="活跃用户数量" icon="heart" v-if="config.database">{{ meta.activeUsers }}</k-numeric>
    <k-numeric title="活跃群数量" icon="users" v-if="config.database">{{ meta.activeGroups }}</k-numeric>
  </div>
  <template v-if="config.database">
    <load-chart/>
    <div class="card-grid chart-grid">
      <history-chart/>
      <hour-chart/>
      <group-chart/>
    </div>
  </template>
</template>

<script setup lang="ts">

import { computed } from 'vue'
import { stats, profile, meta, registry } from '~/client'
import GroupChart from './group-chart.vue'
import HistoryChart from './history-chart.vue'
import HourChart from './hour-chart.vue'
import LoadChart from './load-chart.vue'

const config = KOISHI_CONFIG

const currentRate = computed(() => {
  return profile.value.bots.reduce((sum, bot) => sum + bot.currentRate[0], 0)
})

const recentRate = computed(() => {
  return Object.values(stats.value.botSend).reduce((sum, value) => sum + value, 0).toFixed(1)
})

</script>

<style lang="scss">

</style>
