<script setup lang="ts">
import { ref, onMounted } from 'vue';


const ipData = ref<any>(null);
const loading = ref(true);


const fetchIPData = async () => {
  loading.value = true;
  try {
    
    const response = await fetch('https://ipapi.co/json/');
    if (!response.ok) throw new Error('网络请求失败');
    ipData.value = await response.json();
  } catch (error) {
    console.error("获取数据出错:", error);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchIPData();
});
</script>

<template>
  <div class="container">
    <div class="card">
      <h2 class="title">🚀 访客地理信息卡片</h2>

      <div v-if="loading" class="status-msg">
        <div class="spinner"></div>
        <p>正在定位您的网络坐标...</p>
      </div>

      <div v-else-if="ipData" class="content">
        <div class="ip-section">
          <span class="label">PUBLIC IP ADDRESS</span>
          <h1 class="ip-address">{{ ipData.ip }}</h1>
        </div>

        <div class="info-grid">
          <div class="item">
            <span class="icon">🌍</span>
            <div class="text">
              <label>国家/地区</label>
              <p>{{ ipData.country_name }} ({{ ipData.country_code }})</p>
            </div>
          </div>

          <div class="item">
            <span class="icon">🏙️</span>
            <div class="text">
              <label>城市</label>
              <p>{{ ipData.city || '未知' }}</p>
            </div>
          </div>

          <div class="item">
            <span class="icon">📍</span>
            <div class="text">
              <label>经纬度坐标</label>
              <p>{{ ipData.latitude.toFixed(4) }}, {{ ipData.longitude.toFixed(4) }}</p>
            </div>
          </div>

          <div class="item">
            <span class="icon">📡</span>
            <div class="text">
              <label>互联网服务商 (ISP)</label>
              <p>{{ ipData.org }}</p>
            </div>
          </div>
        </div>

        <div class="actions">
          <a 
            :href="`https://www.google.com/maps/search/?api=1&query=${ipData.latitude},${ipData.longitude}`" 
            target="_blank" 
            class="btn map-btn"
          >
            🗺️ 在地图上查看
          </a>
          <button @click="fetchIPData" class="btn refresh-btn">
            🔄 刷新数据
          </button>
        </div>
      </div>

      <div v-else class="status-msg error">
        <p>无法获取数据，请检查网络连接或稍后重试。</p>
        <button @click="fetchIPData" class="btn refresh-btn">重试</button>
      </div>
    </div>
    
    <p class="footer">Built with Vue3 + Vite | Deployed on GitHub Pages</p>
  </div>
</template>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #f8fafc;
  padding: 20px;
  font-family: 'Inter', -apple-system, sans-serif;
}

/* 卡片样式 */
.card {
  width: 100%;
  max-width: 450px;
  background: white;
  border-radius: 20px;
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
  padding: 32px;
  border: 1px solid #e2e8f0;
}

.title {
  text-align: center;
  color: #1e293b;
  margin-bottom: 24px;
  font-size: 1.25rem;
}

/* IP 地址区 */
.ip-section {
  text-align: center;
  margin-bottom: 24px;
  padding: 16px;
  background: #f1f5f9;
  border-radius: 12px;
}

.label {
  font-size: 0.7rem;
  font-weight: 700;
  color: #64748b;
  letter-spacing: 0.1em;
}

.ip-address {
  font-size: 1.8rem;
  color: #2563eb;
  margin-top: 4px;
  font-family: 'Monaco', 'Courier New', monospace;
}

/* 网格信息区 */
.info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 30px;
}

.item {
  display: flex;
  align-items: flex-start;
  gap: 10px;
}

.icon {
  font-size: 1.2rem;
}

.text label {
  display: block;
  font-size: 0.75rem;
  color: #94a3b8;
  margin-bottom: 2px;
}

.text p {
  font-size: 0.9rem;
  font-weight: 600;
  color: #334155;
  margin: 0;
}

/* 按钮组 */
.actions {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.btn {
  display: block;
  text-align: center;
  padding: 12px;
  border-radius: 10px;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
  text-decoration: none;
  border: none;
}

.map-btn {
  background: #1e293b;
  color: white;
}

.map-btn:hover { background: #0f172a; }

.refresh-btn {
  background: white;
  color: #1e293b;
  border: 1px solid #e2e8f0;
}

.refresh-btn:hover { background: #f8fafc; }

/* 状态展示 */
.status-msg {
  text-align: center;
  padding: 40px 0;
  color: #64748b;
}

.spinner {
  width: 30px;
  height: 30px;
  border: 3px solid #f3f3f3;
  border-top: 3px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.footer {
  margin-top: 20px;
  font-size: 0.75rem;
  color: #94a3b8;
}
</style>
