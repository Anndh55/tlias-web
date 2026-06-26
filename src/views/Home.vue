<template>
  <div class="dashboard">
    <div class="page-head">
      <h2 class="page-title">系统概览</h2>
      <p class="page-desc">教学资源总览与快捷入口</p>
    </div>

    <!-- 统计卡片 -->
    <div class="stats-grid">
      <div class="stat-tile" v-for="s in statItems" :key="s.label">
        <div class="stat-icon" :style="{ background: s.iconBg, color: s.iconColor }">
          <el-icon :size="24">
            <component :is="s.icon" />
          </el-icon>
        </div>
        <div class="stat-info">
          <span class="stat-value">{{ s.count }}</span>
          <span class="stat-label">{{ s.label }}</span>
        </div>
      </div>
    </div>

    <!-- 快速入口 -->
    <div class="actions-section">
      <h3 class="section-title">快速入口</h3>
      <div class="actions-grid">
        <div
          class="action-tile"
          v-for="act in actions"
          :key="act.label"
          @click="$router.push(act.path)"
        >
          <div class="action-icon" :style="{ background: act.bg }">
            <el-icon :size="20" :color="act.color">
              <component :is="act.icon" />
            </el-icon>
          </div>
          <div class="action-info">
            <span class="action-label">{{ act.label }}</span>
            <span class="action-hint">{{ act.hint }}</span>
          </div>
          <el-icon class="action-arrow" :size="16"><ArrowRight /></el-icon>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { listDeptAPI } from '../api/dept.js'
import { listEmpAPI } from '../api/emp.js'
import { listClazzAPI } from '../api/clazz.js'
import { listStudentAPI } from '../api/student.js'
import { Folder, User, School, UserFilled, ArrowRight } from '@element-plus/icons-vue'

const router = useRouter()

const stats = reactive({
  deptCount: 0,
  empCount: 0,
  clazzCount: 0,
  studentCount: 0
})

const statItems = computed(() => [
  { icon: Folder, iconBg: 'rgba(64, 158, 255, 0.10)', iconColor: '#409eff', count: stats.deptCount, label: '部门数' },
  { icon: User, iconBg: 'rgba(47, 84, 235, 0.10)', iconColor: '#2f54eb', count: stats.empCount, label: '员工数' },
  { icon: School, iconBg: 'rgba(82, 196, 26, 0.10)', iconColor: '#52c41a', count: stats.clazzCount, label: '班级数' },
  { icon: UserFilled, iconBg: 'rgba(250, 140, 22, 0.10)', iconColor: '#fa8c16', count: stats.studentCount, label: '学员数' }
])

const actions = [
  { icon: Folder, bg: 'rgba(64, 158, 255, 0.08)', color: '#409eff', label: '部门管理', hint: '管理组织架构与部门信息', path: '/depts' },
  { icon: User, bg: 'rgba(47, 84, 235, 0.08)', color: '#2f54eb', label: '员工管理', hint: '查看和编辑员工档案', path: '/emps' },
  { icon: School, bg: 'rgba(82, 196, 26, 0.08)', color: '#52c41a', label: '班级管理', hint: '管理班级信息与配置', path: '/clazzs' },
  { icon: UserFilled, bg: 'rgba(250, 140, 22, 0.08)', color: '#fa8c16', label: '学员管理', hint: '跟踪学员数据与状态', path: '/students' }
]

onMounted(async () => {
  try {
    const [deptRes, empRes, clazzRes, stuRes] = await Promise.all([
      listDeptAPI(), listEmpAPI(), listClazzAPI(), listStudentAPI()
    ])
    if (deptRes.code === 1) stats.deptCount = deptRes.data?.length || 0
    if (empRes.code === 1) stats.empCount = empRes.data?.length || 0
    if (clazzRes.code === 1) stats.clazzCount = clazzRes.data?.length || 0
    if (stuRes.code === 1) stats.studentCount = stuRes.data?.length || 0
  } catch (e) {
    // ignore
  }
})
</script>

<style scoped>
.dashboard {
  font-family: 'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif;
}

/* ---------- Page header ---------- */
.page-head {
  margin-bottom: 24px;
}

.page-title {
  font-size: 22px;
  font-weight: 700;
  color: #1d1e2c;
  margin: 0 0 4px;
}

.page-desc {
  font-size: 13px;
  color: #8b8fa3;
  margin: 0;
}

/* ---------- Stats grid ---------- */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  margin-bottom: 28px;
}

.stat-tile {
  background: rgba(255, 255, 255, 0.75);
  backdrop-filter: blur(12px) saturate(1.3);
  -webkit-backdrop-filter: blur(12px) saturate(1.3);
  border: 1px solid rgba(255, 255, 255, 0.6);
  border-radius: 16px;
  padding: 22px 24px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: all 0.2s ease;
  cursor: default;
}

.stat-tile:hover {
  background: rgba(255, 255, 255, 0.88);
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.05);
  transform: translateY(-2px);
}

.stat-icon {
  width: 48px;
  height: 48px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.stat-info {
  display: flex;
  flex-direction: column;
}

.stat-value {
  font-size: 28px;
  font-weight: 700;
  color: #1d1e2c;
  line-height: 1.2;
}

.stat-label {
  font-size: 13px;
  color: #8b8fa3;
  margin-top: 2px;
}

/* ---------- Quick entry ---------- */
.actions-section {
  margin-top: 4px;
}

.section-title {
  font-size: 15px;
  font-weight: 600;
  color: #303133;
  margin: 0 0 14px;
}

.actions-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
}

.action-tile {
  background: rgba(255, 255, 255, 0.65);
  backdrop-filter: blur(10px) saturate(1.2);
  -webkit-backdrop-filter: blur(10px) saturate(1.2);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 14px;
  padding: 20px 18px;
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.action-tile:hover {
  background: rgba(255, 255, 255, 0.85);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
  transform: translateY(-2px);
}

.action-icon {
  width: 40px;
  height: 40px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.action-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.action-label {
  font-size: 14px;
  font-weight: 600;
  color: #303133;
}

.action-hint {
  font-size: 12px;
  color: #8b8fa3;
  margin-top: 2px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.action-arrow {
  color: #c0c4cc;
  flex-shrink: 0;
  transition: transform 0.2s ease, color 0.2s ease;
}

.action-tile:hover .action-arrow {
  transform: translateX(3px);
  color: #409eff;
}

/* ---------- Responsive ---------- */
@media (max-width: 1200px) {
  .stats-grid { grid-template-columns: repeat(2, 1fr); }
  .actions-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 640px) {
  .stats-grid { grid-template-columns: 1fr; }
  .actions-grid { grid-template-columns: 1fr; }
}
</style>