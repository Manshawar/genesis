# --init 协议

## 概述

`--init` 是 genesis skill 的首次启动流程。目标：在用户引导下，分层精读项目，生成 `.genesis/` 索引。

## 流程

### Phase 1: 扫描与选择

1. **扫描顶层目录**：执行 `ls -d */` 或等效命令，列出项目根目录下的一级子目录
2. **展示候选区块**：将目录按语义分组展示给用户，例如：
   - `src/pages/`、`src/components/`、`src/store/`、`src/hooks/`、`src/utils/`、`src/api/` → 前端
   - `server/`、`api/`、`internal/`、`cmd/` → 后端（Go）
   - `src/main/java/`、`src/main/resources/` → 后端（Java）
3. **用户选择核心区块**：用户勾选需要精读的目录。前端目录优先展示。后端目录可选。
4. **用户确认技术栈**：
   - 前端：React / Vue / 无
   - 后端：Go / Java / Node.js / 无
   - 从 `package.json`、`go.mod`、`pom.xml` 等文件中自动检测并提示确认

### Phase 2: 骨架精读（第一层）

目标：建立全局地图，不深入模块内部。

1. **前端**：
   - 读路由文件（`router/index.ts` 或等效），提取路由表
   - 读状态管理入口（`store/index.ts` 或等效），提取 store 模块列表
   - 读 API 封装层（`api/` 或 `services/`），提取 baseURL、拦截器
2. **后端**：
   - 读路由注册文件，提取 endpoint 列表
   - 读 service 层入口，提取 service 模块列表
   - 读数据层（model/repo），提取数据表/实体列表
3. **公共**：
   - 读 `package.json` / `go.mod` / `pom.xml`，提取关键依赖
   - 读全局类型定义文件
   - 读公共组件/utils 目录

### Phase 3: 生成索引

1. 生成 `_index.md`：目录树 + 每个模块一句话职责
2. 生成 `_config.md`：核心区块 + 技术栈
3. 生成 `shared/*.md`：公共组件、utils、类型、API 封装
4. 生成 `frontend/_overview.md`：路由表 + 状态管理 + 组件树
5. 生成 `backend/_overview.md`（如果用户选择了后端）
6. 生成 `patterns/`：根据技术栈生成对应模板

### Phase 4: 模块精读（第二层，可选）

Phase 3 完成后询问用户：是否需要立即深入精读某些核心模块？如果用户选择是，对指定模块执行：

1. 读模块目录结构
2. 提取组件列表/API 端点/service 方法
3. 生成 `frontend/{module}.md` 或 `backend/{module}.md`

### Phase 5: 告知覆盖范围

列出已生成的所有索引文件，告知用户当前索引覆盖了哪些区块、哪些区块尚未覆盖。提示用户后续可以用 `--reconfig` 调整核心区块。

## 索引页面模板

### _index.md 模板

```markdown
# 项目全局地图

## 项目概况
- 名称：
- 技术栈：前端 {React/Vue} + 后端 {Go/Java/Node.js}

## 目录结构
\```
项目/
├── src/
│   ├── pages/      — 页面
│   ├── components/ — 公共组件
│   ├── store/      — 状态管理
│   ├── api/        — API 封装
│   └── utils/      — 工具函数
├── server/         — 后端服务
\```

## 模块索引
### 前端
- [[frontend/auth]] — 认证模块：登录/注册/权限
- [[frontend/dashboard]] — 仪表盘
- ...

### 后端
- [[backend/user-service]] — 用户服务
- ...
```

### frontend/{module}.md 模板

```markdown
# {模块名}

## 职责
一句话描述这个模块是干什么的

## 路由
- `/path` — 页面名

## 页面列表
- `PageName` (`src/pages/xxx/index.tsx`) — 页面职责

## 状态
- 使用的 store：[[shared/store#xxx]]
- 使用的 hooks：[[shared/hooks#xxx]]

## API 调用
- `GET /api/xxx` — [[backend/xxx-service#endpoint]]
- `POST /api/yyy` — [[backend/yyy-service#endpoint]]

## 关联模块
- [[frontend/xxx]] — 关联原因
```

### _config.md 模板

```markdown
# 项目配置

## 核心区块
- src/pages/
- src/components/
- src/store/
- src/api/
- server/internal/

## 技术栈
- 前端：React 18 + TypeScript
- 后端：Go 1.21 + Gin
- 状态管理：Zustand
- 路由：React Router v6

## 未覆盖区块
- src/__tests__/（测试文件，暂不索引）
- docs/（文档，暂不索引）
```