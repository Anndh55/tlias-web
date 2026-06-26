<template>
  <div class="layout-container">
    <!-- 侧边栏 -->
    <div class="sidebar" :class="{ collapsed: isCollapse }" :style="{ width: isCollapse ? '64px' : '220px' }">
      <div class="sidebar-header">
        <div class="brand-icon">
          <svg width="32" height="32" viewBox="0 0 40 40" fill="none">
            <rect width="40" height="40" rx="10" fill="#409eff"/>
            <path d="M12 20L18 26L28 14" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
        <span v-show="!isCollapse" class="logo-text">智学云帆</span>
      </div>
      <el-menu
        :default-active="activeMenu"
        :collapse="isCollapse"
        :router="true"
      >
        <el-menu-item index="/home">
          <el-icon><HomeFilled /></el-icon>
          <template #title>首页</template>
        </el-menu-item>
        <el-sub-menu index="content">
          <template #title>
            <el-icon><Management /></el-icon>
            <span>内容管理</span>
          </template>
          <el-menu-item index="/depts">
            <el-icon><Folder /></el-icon>
            <template #title>部门管理</template>
          </el-menu-item>
          <el-menu-item index="/emps">
            <el-icon><User /></el-icon>
            <template #title>员工管理</template>
          </el-menu-item>
          <el-sub-menu index="clazz-student">
            <template #title>
              <el-icon><Reading /></el-icon>
              <span>班级学员管理</span>
            </template>
            <el-menu-item index="/clazzs">
              <el-icon><School /></el-icon>
              <template #title>班级管理</template>
            </el-menu-item>
            <el-menu-item index="/students">
              <el-icon><UserFilled /></el-icon>
              <template #title>学员管理</template>
            </el-menu-item>
          </el-sub-menu>
        </el-sub-menu>
      </el-menu>
    </div>

    <!-- 主区域 -->
    <div class="main-area">
      <!-- 顶部导航 -->
      <div class="topbar">
        <div class="topbar-left">
          <el-icon class="collapse-btn" @click="isCollapse = !isCollapse">
            <Fold v-if="!isCollapse" />
            <Expand v-else />
          </el-icon>
          <span class="breadcrumb">{{ currentTitle }}</span>
        </div>
        <div class="topbar-right">
          <span class="user-info">
            <el-icon><Avatar /></el-icon>
            {{ user?.name || '未登录' }}
          </span>
          <el-button class="logout-btn" size="small" @click="handleLogout">退出登录</el-button>
        </div>
      </div>

      <!-- 内容区 -->
      <div class="content">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()
const isCollapse = ref(false)

const user = JSON.parse(localStorage.getItem('user') || '{}')

const activeMenu = computed(() => route.path)

const currentTitle = computed(() => route.meta?.title || '首页')

const handleLogout = () => {
  localStorage.removeItem('token')
  localStorage.removeItem('user')
  router.push('/login')
}
</script>

<style scoped>
.layout-container {
  display: flex;
  height: 100vh;
  overflow: hidden;
  font-family: 'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif;
}

.sidebar {
  position: relative;
  background: rgba(255, 255, 255, 0.78);
  backdrop-filter: blur(20px) saturate(1.3);
  -webkit-backdrop-filter: blur(20px) saturate(1.3);
  transition: width 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  overflow-y: auto;
  overflow-x: hidden;
  flex-shrink: 0;
  z-index: 10;
  border-right: 1px solid rgba(0, 0, 0, 0.06);
}

.sidebar-header {
  height: 56px;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 16px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.sidebar.collapsed .sidebar-header {
  justify-content: center;
  padding: 0;
}

.brand-icon {
  display: flex;
  align-items: center;
  flex-shrink: 0;
}

.logo-text {
  font-size: 18px;
  font-weight: 700;
  color: #1d1e2c;
  letter-spacing: 2px;
  white-space: nowrap;
}

.sidebar .el-menu {
  border-right: none;
  background: transparent;
}

.sidebar .el-menu .el-menu-item,
.sidebar .el-menu .el-sub-menu__title {
  background: transparent !important;
  color: #4a4d5e;
  transition: background 0.15s ease, color 0.15s ease;
}

.sidebar .el-menu .el-menu-item:hover,
.sidebar .el-menu .el-sub-menu__title:hover {
  background: rgba(64, 158, 255, 0.06) !important;
  color: #409eff;
}

.sidebar .el-menu .el-menu-item.is-active {
  color: #409eff;
  background: rgba(64, 158, 255, 0.10) !important;
  position: relative;
}

.sidebar .el-menu .el-menu-item.is-active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 20px;
  background: #409eff;
  border-radius: 0 3px 3px 0;
}

.sidebar .el-menu .el-menu-item .el-icon,
.sidebar .el-menu .el-sub-menu__title .el-icon {
  color: #6b6f82;
}

.sidebar .el-menu .el-menu-item.is-active .el-icon,
.sidebar .el-menu .el-menu-item:hover .el-icon,
.sidebar .el-menu .el-sub-menu__title:hover .el-icon {
  color: #409eff;
}

.sidebar .el-menu--collapse .el-sub-menu .el-menu {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-radius: 8px;
  border: 1px solid rgba(0, 0, 0, 0.06);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.06);
}

.main-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  background: #f0f4f8;
}

.topbar {
  position: relative;
  height: 56px;
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: blur(16px) saturate(1.3);
  -webkit-backdrop-filter: blur(16px) saturate(1.3);
  border-bottom: 1px solid rgba(0, 0, 0, 0.06);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  flex-shrink: 0;
  z-index: 5;
}

.topbar-left {
  display: flex;
  align-items: center;
  gap: 14px;
}

.collapse-btn {
  font-size: 20px;
  cursor: pointer;
  color: #8b8fa3;
  transition: color 0.15s ease, transform 0.15s ease;
}
.collapse-btn:hover {
  color: #409eff;
  transform: scale(1.05);
}

.breadcrumb {
  font-size: 15px;
  font-weight: 600;
  color: #303133;
}

.topbar-right {
  display: flex;
  align-items: center;
  gap: 20px;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 14px;
  color: #606266;
}

.user-info .el-icon {
  font-size: 18px;
  color: #8b8fa3;
}

.logout-btn {
  border-radius: 8px;
  font-family: inherit;
  transition: all 0.15s ease;
}

.content {
  flex: 1;
  padding: 24px;
  overflow-y: auto;
}
</style>