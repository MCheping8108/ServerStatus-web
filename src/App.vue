<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import { ElMessage } from 'element-plus'

const data = ref(null)
// const getJson = axios.get('/api').then((res) => {
//   data.value = res.data
//   console.log(data.value)
// })
setInterval(() => {
  axios
    .get('/api')
    .then((res) => {
      data.value = res.data
      console.log(data.value)
    })
    .catch((err) => {
      ElMessage.error('请求失败，原因：' + err.message)
      return
    })
}, 2000)
// !请求成功:
// Proxy { <target>: {…}, <handler>: {…} }
// ​
// <target>: Object { onlineMode: true, cpuUsage: 0.041975611919315536, motd: "A Minecraft Server", … }
// ​
// <handler>: Object { _isReadonly: false, _isShallow: false }
</script>

<template>
  <div class="main-glass-container">
    <div class="container">
      <div class="progress-card">
        <el-progress type="dashboard" :percentage="(data as any)?.cpuUsage * 100" :stroke-width="18" :width="200">
          <template #default="{ percentage }">
            <span class="percentage-value">{{ percentage.toFixed(2) }}%</span>
            <span class="percentage-label">CPU</span>
          </template>
        </el-progress>
      </div>

      <div class="progress-card">
        <el-progress type="dashboard" :percentage="((data as any)?.memUsage / 1000000000 / 8) * 100" :stroke-width="18"
          :width="200">
          <template #default="{ percentage }">
            <span class="percentage-value">{{ ((percentage / 100) * 8).toFixed(2) }}GB</span>
            <span class="percentage-label">Memory</span>
          </template>
        </el-progress>
      </div>

      <div class="progress-card">
        <el-progress type="dashboard" :stroke-width="18" :width="200">
          <template #default="{ percentage }">
            <span class="percentage-value">{{ (data as any)?.playerCount }} / {{ (data as any)?.playerMax }}</span>
            <span class="percentage-label">Players</span>
          </template>
        </el-progress>
      </div>
    </div>

    <div class="info-container">
      <el-result :icon="(data as any)?.onlineMode ? 'success' : 'error'" title="服务器正版验证状态"
        :sub-title="(data as any)?.onlineMode ? '该服务器已启动正版验证' : '该服务器未启动正版验证'">
      </el-result>
      <h1>服务器简介：{{ (data as any)?.motd }}</h1>
      <h1>服务器版本：{{ (data as any)?.version }}</h1>
      <h1>服务器名称：{{ (data as any)?.serverName }}</h1>
    </div>
  </div>
</template>

<style scoped></style>
