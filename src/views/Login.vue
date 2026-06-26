<template>
  <div class="login-wrapper">
    <div class="login-bg">
      <div class="bg-grid" />
      <div class="bg-glow bg-glow-1" />
      <div class="bg-glow bg-glow-2" />
    </div>
    <div class="login-container">
      <div class="login-card">
        <div class="login-header">
          <div class="brand-icon">
            <svg width="40" height="40" viewBox="0 0 40 40" fill="none">
              <rect width="40" height="40" rx="10" fill="#409eff"/>
              <path d="M12 20L18 26L28 14" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <h2 class="login-title">智学云帆</h2>
          <p class="login-subtitle">教学管理系统</p>
        </div>
        <el-form ref="formRef" :model="form" :rules="rules" size="large" class="login-form" @keyup.enter="handleLogin">
          <el-form-item prop="username">
            <el-input v-model="form.username" placeholder="员工用户名" :prefix-icon="User" />
          </el-form-item>
          <el-form-item prop="password">
            <el-input v-model="form.password" type="password" placeholder="密码" :prefix-icon="Lock" show-password />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" :loading="loading" class="login-btn" @click="handleLogin">
              {{ loading ? '登录中...' : '登 录' }}
            </el-button>
          </el-form-item>
        </el-form>
      </div>
      <p class="login-footer">智学云帆 v1.0</p>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'
import { User, Lock } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import { loginAPI } from '../api/login'

const router = useRouter()
const formRef = ref(null)
const loading = ref(false)

const form = reactive({
  username: '',
  password: ''
})

const rules = {
  username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
  password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
}

const handleLogin = async () => {
  const valid = await formRef.value.validate().catch(() => false)
  if (!valid) return

  loading.value = true
  try {
    const res = await loginAPI(form)
    if (res.code === 1) {
      localStorage.setItem('token', res.data.token)
      localStorage.setItem('user', JSON.stringify({
        id: res.data.id,
        username: res.data.username,
        name: res.data.name
      }))
      ElMessage.success('登录成功')
      router.push('/')
    }
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.login-wrapper {
  position: relative;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background: linear-gradient(160deg, #f0f4f8 0%, #e8eef5 40%, #f5f7fa 100%);
  font-family: 'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif;
}

.login-bg {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}

.bg-grid {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(64, 158, 255, 0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(64, 158, 255, 0.04) 1px, transparent 1px);
  background-size: 48px 48px;
}

.bg-glow {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.35;
}

.bg-glow-1 {
  width: 400px;
  height: 400px;
  top: -10%;
  right: -5%;
  background: radial-gradient(circle, #409eff 0%, transparent 70%);
}

.bg-glow-2 {
  width: 350px;
  height: 350px;
  bottom: -8%;
  left: -5%;
  background: radial-gradient(circle, #79bbff 0%, transparent 70%);
}

.login-container {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.login-card {
  width: 400px;
  padding: 44px 40px 36px;
  background: rgba(255, 255, 255, 0.75);
  backdrop-filter: blur(24px) saturate(1.4);
  -webkit-backdrop-filter: blur(24px) saturate(1.4);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.6);
  box-shadow:
    0 4px 24px rgba(0, 0, 0, 0.04),
    0 1px 2px rgba(0, 0, 0, 0.02),
    inset 0 1px 0 rgba(255, 255, 255, 0.8);
  transition: box-shadow 0.25s ease;
}

.login-card:hover {
  box-shadow:
    0 8px 40px rgba(0, 0, 0, 0.06),
    0 1px 3px rgba(0, 0, 0, 0.03),
    inset 0 1px 0 rgba(255, 255, 255, 0.8);
}

.login-header {
  text-align: center;
  margin-bottom: 32px;
}

.brand-icon {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
}

.login-title {
  font-size: 26px;
  font-weight: 700;
  color: #1d1e2c;
  margin: 0 0 6px;
  letter-spacing: 3px;
}

.login-subtitle {
  font-size: 14px;
  color: #8b8fa3;
  margin: 0;
  font-weight: 400;
}

.login-form {
  margin-top: 4px;
}

.login-form :deep(.el-input__wrapper) {
  background: rgba(255, 255, 255, 0.7);
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.06) inset;
  border-radius: 10px;
  padding: 4px 12px;
  transition: box-shadow 0.2s ease, background 0.2s ease;
}

.login-form :deep(.el-input__wrapper:hover) {
  box-shadow: 0 0 0 1px rgba(64, 158, 255, 0.3) inset;
}

.login-form :deep(.el-input__wrapper.is-focus) {
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2) inset;
  background: rgba(255, 255, 255, 0.9);
}

.login-form :deep(.el-input__inner) {
  font-size: 15px;
  height: 42px;
  font-family: inherit;
}

.login-form :deep(.el-input__prefix) {
  margin-right: 8px;
}

.login-form :deep(.el-input__prefix-inner) {
  color: #8b8fa3;
}

.login-form :deep(.el-form-item) {
  margin-bottom: 20px;
}

.login-form :deep(.el-form-item:last-child) {
  margin-bottom: 0;
}

.login-btn {
  width: 100%;
  height: 46px;
  font-size: 16px;
  font-weight: 600;
  border-radius: 10px;
  letter-spacing: 6px;
  border: none;
  transition: transform 0.15s ease, box-shadow 0.2s ease;
  font-family: inherit;
}

.login-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(64, 158, 255, 0.3);
}

.login-btn:active {
  transform: translateY(0);
  box-shadow: none;
}

.login-footer {
  margin-top: 20px;
  font-size: 12px;
  color: #b0b4c0;
  letter-spacing: 1px;
}
</style>
