### 目的

本文件面向自动化代码代理（Agent）与新加入的贡献者，提供本仓库的**结构、常用命令、约定与注意事项**，以便在不额外沟通的情况下完成改动并自检。

### 仓库概览

本仓库包含两个相互独立的 Node.js 子项目：

- **`koa-server/`**：Koa 服务器（CommonJS），提供简单健康检查接口。
- **`react18-vite/`**：React + TypeScript + Vite 前端模板项目，带 ESLint 配置。

### 目录结构

- **`README.md`**：根 README（目前内容较少）
- **`koa-server/`**
  - `src/index.js`：Koa 入口，默认端口 `3000`，提供 `GET /health`
  - `package.json` / `package-lock.json`
- **`react18-vite/`**
  - `src/`：前端源码（`main.tsx`、`App.tsx` 等）
  - `vite.config.ts`：Vite 配置（默认配置）
  - `eslint.config.js`：ESLint（flat config）
  - `package.json` / `package-lock.json`

### 环境要求

- **Node.js + npm**：优先使用与 `package-lock.json` 兼容的 npm（一般为 npm v9+）。
- **系统**：Linux/WSL/macOS 均可。

### 依赖安装

在各自子目录内安装依赖（推荐 `npm ci` 以保证与 lockfile 一致）：

- **Koa 服务端**

```bash
cd koa-server
npm ci
```

- **React 前端**

```bash
cd react18-vite
npm ci
```

### 本地运行

#### `koa-server/`（服务端）

- **开发启动**

```bash
cd koa-server
npm run dev
```

- **生产启动**

```bash
cd koa-server
npm run start
```

- **端口/环境变量**
  - **`PORT`**：可覆盖端口；默认 `3000`

- **健康检查**
  - `GET /health` 返回 `{ ok: true }`

#### `react18-vite/`（前端）

- **开发启动（HMR）**

```bash
cd react18-vite
npm run dev
```

Vite 默认监听端口通常为 **`5173`**（如端口被占用会自动顺延）。

- **本地预览**

```bash
cd react18-vite
npm run build
npm run preview
```

### 代码检查与构建（Agent 必做）

在提交改动前，尽量运行与改动相关的最小集合检查：

- **前端 ESLint**

```bash
cd react18-vite
npm run lint
```

- **前端构建**

```bash
cd react18-vite
npm run build
```

> `koa-server/` 当前没有可用的测试脚本（`npm test` 会直接以退出码 1 失败），除非你同时把测试脚本补齐，否则不要把它当作必跑项。

### 修改约定（面向 Agent）

- **尽量小步改动**：优先做最小可行修复/增强，避免不相关的格式化与重构。
- **依赖与锁文件**
  - 使用 `npm` 管理依赖；新增/升级依赖时应同步更新对应子项目的 `package-lock.json`。
  - 不要在根目录新增全局 `package.json`，除非明确要引入 monorepo 管理。
- **端口与跨域**
  - `koa-server` 已启用 `@koa/cors`；如需与前端联调，优先通过环境变量与文档说明端口。
- **可复现的命令**：在 PR/变更说明里写清楚你运行过的命令（例如 `npm run lint`、`npm run build`）。

### 常见排错

- **`npm ci` 失败**：确认 Node/npm 版本与 `package-lock.json` 生成时相近；必要时只在对应子项目内重新生成 lockfile（并在变更中说明原因）。
- **端口占用**：
  - Koa：设置 `PORT=3001 npm run dev`
  - Vite：使用 `--port`（例如 `npm run dev -- --port 5174`）

