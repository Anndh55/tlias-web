---
name: 智学云帆
description: 教学管理系统
colors:
  sidebar-bg: "#1d1e2c"
  sidebar-text: "#bfcbd9"
  sidebar-active: "#409eff"
  topbar-bg: "#ffffff"
  content-bg: "#f5f7fa"
  body-text: "#333333"
  primary: "#409eff"
  heading: "#303133"
  secondary-text: "#606266"
  muted-text: "#909399"
typography:
  body:
    fontFamily: "'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif"
    fontSize: "14px"
    fontWeight: 400
    lineHeight: 1.6
  heading:
    fontFamily: "'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif"
    fontSize: "18px"
    fontWeight: 600
    lineHeight: 1.4
  display:
    fontFamily: "'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif"
    fontSize: "28px"
    fontWeight: 700
    lineHeight: 1.3
    letterSpacing: "0.02em"
  label:
    fontFamily: "'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', -apple-system, sans-serif"
    fontSize: "13px"
    fontWeight: 500
    lineHeight: 1.4
rounded:
  sm: "4px"
  md: "8px"
  lg: "12px"
spacing:
  xs: "8px"
  sm: "12px"
  md: "16px"
  lg: "20px"
components:
  button-primary:
    backgroundColor: "#409eff"
    textColor: "#ffffff"
    rounded: "4px"
    padding: "8px 20px"
  input-default:
    backgroundColor: "#ffffff"
    textColor: "#333333"
    rounded: "4px"
    padding: "8px 12px"
  card-default:
    backgroundColor: "#ffffff"
    textColor: "#333333"
    rounded: "0px"
---

# Design System: 智学云帆

## 1. Overview

**Creative North Star: "The Glass Office"**

一个像玻璃办公室一样的后台管理系统——简洁、透明、层次分明。浅色主调，线条利落，光影细腻。不靠卡片堆叠来建立视觉结构，而是靠层级、间距、薄边框和温柔阴影。不用多余装饰，但每一处细节都经过设计。信息密度适中，操作直觉，像一个精密的教学管理工具。

**关键特征：**
- 浅色主调，白色、浅灰、微薄蓝色温和中性色
- 层次通过阴影和叠层而非卡片框架区分
- 线条利落的边框（细线，1px）
- 全站中文字体统一，清晰可读
- 细腻的光影语言，非重阴影也非完全平面
- 操作区域一目了然，减少认知负担

## 2. Colors: The Glass Office Palette

当前的色彩体系基于Element Plus默认配色，进行了简单自定义：深蓝侧边栏（#1d1e2c）搭配白色顶栏和浅灰内容区（#f5f7fa）。主色为Element Plus默认蓝（#409eff）。登录页使用深色渐变背景。

### Primary
- **Element Blue** (#409eff): 全站主色，用于按钮、活跃状态、链接。

### Neutral
- **Navy Sidebar** (#1d1e2c): 侧边栏背景
- **White Topbar** (#ffffff): 顶部导航栏
- **Light Gray Background** (#f5f7fa): 内容区基础背景
- **Dark Text** (#333333): 正文文字
- **Secondary Text** (#606266): 副标签、提示文本
- **Heading** (#303133): 标题文字

### Named Rules
**The Glass Surface Rule.** 背景层不超过3级（背景、表面、弹出）。每一级通过微弱阴影或薄边框区分，不用卡片套卡片。

## 3. Typography

**Display Font:** Noto Sans SC / 思源黑体
**Body Font:** Noto Sans SC / 思源黑体

**Character:** 全中文字体统一。清晰、专业、不花哨。数字和英文也使用同一字体，保持视觉统一性。

### Hierarchy
- **Display** (700, 28px, 1.3): 页面大标题、登录页标题、统计数值
- **Heading** (600, 18px, 1.4): 区域标题、卡片标题
- **Body** (400, 14px, 1.6): 正文、表格文字、表单标签
- **Label** (500, 13px, 1.4): 小标签、按钮文字、表格列头

### Named Rules
**The No-Serif Rule.** 后台管理系统不使用衬线体。全站仅使用黑体系列。

## 4. Elevation

当前系统使用混合阴影策略：入口卡片有明显阴影（0 8px 40px rgba(0,0,0,0.2)），内容区基本平面，顶栏用边框而非阴影区分。

### The Glass Lift Rule
表面之间的区分优先使用薄边框（1px solid rgba(0,0,0,0.06)）或微弱阴影（0 1px 3px rgba(0,0,0,0.04)）。只有弹窗、下拉菜单等浮动元素使用明显阴影（0 8px 24px rgba(0,0,0,0.08)）。

## 5. Components

### Buttons
- **Shape:** 微圆角（4px）
- **Primary:** 蓝色背景（#409eff）+ 白色文字，内边距8px 20px
- **Danger:** 红色背景（#f56c6c）+ 白色文字
- **Type:** Element Plus默认按钮系统，支持 primary / success / warning / danger / info

### Tables
- **Style:** 白色背景，带网格线梁，支持添加条纹样式
- **Column headers:** 深色文字（#303133），粗体
- **Operations:** 左侧固定，使用链接样式按钮

### Inputs
- **Style:** 白色背景，线框，4px 圆角
- **Focus:** Element Plus默认蓝色边框变化

### Navigation (Sidebar)
- **Style:** 深蓝背景（#1d1e2c），浅灰文字（#bfcbd9）
- **Active:** 蓝色高亮（#409eff）
- **Collapsible:** 220px展开 / 64px收缩

## 6. Do's and Don'ts

### Do:
- **Do** 使用浅色背景搭配细边框，让内容呼吸
- **Do** 在表单、弹窗、叠加层上使用微弱透明效果（backdrop-filter）
- **Do** 保持全站圆角统一（小元素4px，大表面8px）
- **Do** 使用薄边框或微阴影来区分层级，避免卡片堆叠
- **Do** 让表格信息一目了然，操作列紧凑高效

### Don't:
- **Don't** 用大片白卡片套卡片的模板化布局
- **Don't** 使用自带阴影的Element Plus卡片默认样式
- **Don't** 在同一层级混合多种阴影、多种圆角、多种边框风格
- **Don't** 使用衬线体作为标题
- **Don't** 用过度的渐变、玻璃质感或装饰性浮动背景
- **Don't** 把操作按钮隐藏得需要点击才看到
