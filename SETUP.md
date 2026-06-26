# Deploy on Sealos DevBox

## DevBox 部署步骤

前端服务在 DevBox 上是一个独立容器（端口 3000，内网地址 `http://tlias-web.ns-jnrdqssh:3000`）。

### 1. 克隆仓库

进入 DevBox 前端服务的 workspace，**不要使用 Vue 模板**（或用空白模板）：

```bash
git clone https://github.com/Anndh55/tlias-web.git /tmp/tlias-web
cp -r /tmp/tlias-web/* ./
cp -r /tmp/tlias-web/.* ./ 2>/dev/null
rm -rf /tmp/tlias-web
```

如果已经用了模板且 clone 到了子目录 `tlias-web/`，先清理再移出来：

```bash
# 清理模板文件
rm -rf CLAUDE.md README.md entrypoint.sh index.html jsconfig.json package.json package-lock.json vite.config.js assets src public .vscode node_modules AGENTS.md

# 把 tlias-web/ 内容移到根目录
mv tlias-web/* ./
mv tlias-web/.* ./ 2>/dev/null
rmdir tlias-web
```

### 2. 安装依赖

```bash
npm install
```

### 3. 启动

```bash
bash entrypoint.sh
```

前端运行在 `3000` 端口。Vite 代理自动将 `/api` 请求转发到后端内网地址 `http://tlias.ns-jnrdqssh:8080`（配置在 `entrypoint.sh` 的环境变量 `VITE_API_PROXY_TARGET` 中）。

---

## 本地开发

```bash
npm run dev
# 启动在 http://localhost:3000
# Vite 代理将 /api 请求转发到 http://localhost:8080
```

### 环境变量

| 变量 | 说明 | 默认值 |
|---|---|---|
| `VITE_API_PROXY_TARGET` | Vite 代理目标地址 | `http://localhost:8080`（DevBox 上自动设为 `http://tlias.ns-jnrdqssh:8080`） |
