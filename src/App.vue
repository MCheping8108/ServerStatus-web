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
  <div class="container">
    <div class="glass-card">
      <el-progress
        type="dashboard"
        :percentage="data?.cpuUsage * 100"
        :stroke-width="18"
        :width="200"
      >
        <template #default="{ percentage }">
          <span class="percentage-value">{{ percentage.toFixed(2) }}%</span>
          <span class="percentage-label">CPU</span>
        </template>
      </el-progress>
    </div>

    <div class="glass-card">
      <el-progress
        type="dashboard"
        :percentage="(data?.memUsage / 1000000000 / 8) * 100"
        :stroke-width="18"
        :width="200"
      >
        <template #default="{ percentage }">
          <span class="percentage-value">{{ ((percentage / 100) * 8).toFixed(2) }}GB</span>
          <span class="percentage-label">Memory</span>
        </template>
      </el-progress>
    </div>

    <div class="glass-card">
      <el-progress type="dashboard" :stroke-width="18" :width="200">
        <template #default="{ percentage }">
          <span class="percentage-value">{{ data?.playerCount }} / {{ data?.playerMax }}</span>
          <span class="percentage-label">Players</span>
        </template>
      </el-progress>
    </div>
  </div>

  <div class="info-container">
    <h1>正版验证？：{{ data?.onlineMode ? '是' : '否' }}</h1>
    <h1>服务器简介：{{ data?.motd }}</h1>
    <h1>服务器版本：{{ data?.version }}</h1>
  </div>
</template>

<style scoped>
html {
  background: url(https://mydove.peacefuly.top/mydove/NAT.png);
}
body {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 30px;
  flex-wrap: wrap;
  padding: 40px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 60vh;
}

.glass-card {
  backdrop-filter: blur(10px);
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  padding: 30px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.3s ease;
}

.glass-card:hover {
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.percentage-value {
  display: block;
  margin-top: 10px;
  font-size: 28px;
  color: #fff;
  font-weight: bold;
}

.percentage-label {
  display: block;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
  margin-top: 5px;
}

.info-container {
  text-align: center;
  color: white;
  padding: 20px;
}

.info-container h1 {
  margin: 10px 0;
  font-size: 18px;
}
</style>
