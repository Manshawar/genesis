
---

## SKILL.md

```markdown
---
name: work
description: "前后端分离项目的工程开发助手。精读项目生成 .gwork/ 索引，后续写需求、修 bug、写测试时按混合协议检索精准上下文。支持 Forge（新建模块/改架构）和 Quick（照葫芦画瓢）两种模式。前端 React/Vue，后端 Go/Java。触发：开始工作、修 bug、加功能、写测试、重构、读项目。"
argument-hint: [--mode forge|quick] [--init] [--reconfig] [--dry-run] [task]
---

# Work

IRON LAW: 每次工作前必须先读 `.work/_index.md`。不知道项目里有什么就动手，是所有 bug 的源头。

## $ARGUMENTS

| 参数 | 说明 | 默认 |
|------|------|------|
| `<task>` | 任务描述（修 bug、加功能、写测试） | 必填 |
| `--mode forge` | 涉及新建模块/改架构/引入新约定 | 自动判断 |
| `--mode quick` | 在现有模块和约定内修改 | 自动判断 |
| `--init` | 首次精读项目，生成 `.work/` | - |
| `--reconfig` | 重新选择核心区块和技术栈 | - |
| `--dry-run` | 只输出计划，不改代码 | false |

模式自动判断：任务涉及新建路由模块、引入新依赖、改变状态管理方式 → Forge。其他 → Quick。

## 工作流

```
Work Progress:

- [ ] Step 0: 环境检查
  - [ ] .work/_config.md 存在？否 → 提示执行 --init
  - [ ] 运行 git diff --stat，标记 [stale] 的索引页面
- [ ] Step 1: 读索引 ⚠️ REQUIRED
  - [ ] 读 .work/_index.md（全局地图）
  - [ ] 读 .work/_config.md（核心区块 + 技术栈）
  - [ ] 读 .work/_conventions.md（架构约定）
  - [ ] 判断模式并告知用户
- [ ] Step 2: 检索上下文 ⚠️ REQUIRED
  - [ ] 必查层：.work/shared/*.md + .work/patterns/*.md（自动加载）
  - [ ] 前端按需层：根据任务关键字匹配 .work/frontend/*.md
  - [ ] 后端按需层：根据任务关键字匹配 .work/backend/*.md
  - [ ] 沿 _index.md 中的链接定向深入
  - [ ] 汇总：告知用户引用了哪些模块/约定
- [ ] Step 3: 制定变更计划
  - Forge: 列出所有受影响模块和新约定，逐项确认
  - Quick: 列出变更清单，一次确认
- [ ] Step 4: 执行
  - [ ] 读写代码文件
  - [ ] 同步更新 .work/ 索引（受影响页面标记为已更新）
- [ ] Step 5: 报告
  - [ ] 变更的文件数
  - [ ] 新增/删除的模块
  - [ ] 是否引入新约定 → 更新 _conventions.md
```

## --init 流程

详见 `references/init-protocol.md`。核心步骤：

1. 扫描顶层目录，列出候选区块
2. 用户选择核心区块（前端优先，后端可选）
3. 用户确认技术栈（React/Vue/Go/Java）
4. 分两层精读：先骨架（路由 + 目录结构），再肌肉（模块内部）
5. 生成 `.work/` 索引并保存配置
6. 告知用户索引覆盖范围

## .work/ 索引结构

```
.work/
├── _index.md           # 全局地图：目录树 + 每个模块一句话职责
├── _config.md          # 核心区块 + 技术栈（--init 生成，--reconfig 修改）
├── _conventions.md     # 团队架构约定
├── shared/             # 必查层 — 跨端公共资产
│   ├── components.md   # 公共组件清单
│   ├── utils.md        # 工具函数清单
│   ├── types.md        # 全局类型/接口定义
│   └── api-client.md   # API 封装层
├── frontend/           # 前端模块索引（按需层）
│   ├── _overview.md    # 前端总览：路由表 + 状态管理 + 组件树
│   └── {module}.md     # 一个模块一页
├── backend/            # 后端模块索引（按需层，可选）
│   ├── _overview.md    # 后端总览：路由表 + service 层 + 数据层
│   └── {module}.md     # 一个模块一页
└── patterns/           # 必查层 — 约定俗成的写法
    ├── page-template.md      # 页面三板斧
    ├── api-endpoint.md       # API 端点模板
    └── component-pattern.md  # 组件写法模式
```

## 检索协议

详见 `references/retrieval-protocol.md`。核心原则：

1. **Step 1 永远第一步**：读 `_index.md` + `_config.md` + `_conventions.md`
2. **必查层自动加载**：`shared/` + `patterns/` 下所有页面，每次任务都读
3. **按需层关键字匹配**：从任务描述提取模块名/文件名/函数名，匹配 `frontend/` 或 `backend/` 下的页面
4. **沿链接定向深入**：读到任意页面后，沿 `[[links]]` 定向加载被引用的页面
5. **不扫描**：永远不做广度优先的全局搜索

## Forge vs Quick

| | Forge | Quick |
|---|---|---|
| 触发 | 新建路由模块、改架构、改状态管理方式、引入新依赖 | 在现有模块内修改 |
| 确认 | 逐模块/逐约定确认 | 变更清单一键确认 |
| 索引更新 | 可能新建 module 页面、更新 shared/、更新 _conventions.md | 更新已有 module 页面 |

## 增量更新

每次工作前执行 `git diff --stat`。变更文件对应的 `.work/` 索引页面标记为 `[stale]`。若 stale 页面被本次任务命中，先更新索引再执行。

## 反模式

- **不读索引就动手**：不知道项目结构就开始写代码
- **Quick 模式引入新约定**：照葫芦画瓢时偷偷改变了架构约定
- **全量扫描源码**：不读 `.work/` 索引，直接 `grep` 整个项目
- **索引不更新**：改完代码不更新 `.work/`，下次工作索引就是错的
- **忽略 _config.md**：不知道核心区块和技术栈就开始读代码
- **忽略 _conventions.md**：团队有约定但 AI 不读，写出风格不一致的代码
- **跳过 Step 0**：不检查 stale 标记就开始工作，用过期索引做决策
```

---

## references/init-protocol.md

```markdown
# --init 协议

## 概述

`--init` 是 work skill 的首次启动流程。目标：在用户引导下，分层精读项目，生成 `.work/` 索引。

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
```

---

## references/retrieval-protocol.md

```markdown
# 混合检索协议

## 核心原则

1. **索引优先，不扫描源码**
2. **必查层自动加载，按需层关键字匹配**
3. **沿链接定向深入，不做广度搜索**

## 检索步骤

### Step 1: 全局入口（每次必读）

按顺序读取：
1. `.work/_index.md` — 了解项目全貌
2. `.work/_config.md` — 确认核心区块和技术栈
3. `.work/_conventions.md` — 确认架构约定

### Step 2: 自动加载必查层

无论任务是什么，以下页面每次必读：
- `.work/shared/components.md`
- `.work/shared/utils.md`
- `.work/shared/types.md`
- `.work/shared/api-client.md`
- `.work/patterns/page-template.md`
- `.work/patterns/api-endpoint.md`
- `.work/patterns/component-pattern.md`

必查层保证 AI 知道项目的公共基础设施和约定写法，避免：
- 重复造轮子（不知道已有公共组件）
- 风格不一致（不知道团队约定的页面/API/组件写法）
- 破坏全局类型（不知道类型定义在哪）

### Step 3: 按需层关键字匹配

从任务描述中提取关键字（模块名、页面名、API 路径、文件名、函数名），匹配以下目录中的页面标题：

- 前端任务 → 匹配 `frontend/*.md`
- 后端任务 → 匹配 `backend/*.md`
- 全栈任务 → 两者都匹配

匹配规则：
- 精确匹配：任务中的模块名与索引页面标题一致 → 直接加载
- 模糊匹配：任务关键字出现在索引页面的职责描述或关键词标签中 → 加载
- 无匹配：只加载体量最小的 `_overview.md`，不做全量扫描

### Step 4: 沿链接定向深入

读到任意索引页面后，如果其中有 `[[links]]` 指向其他页面：
- 仅当被链接页面的内容与当前任务相关时，才继续加载
- 不做递归展开——最多两层链接
- 判断"相关"的标准：被链接页面的职责描述中出现了任务关键字

### Step 5: 汇总告知

检索完成后，告知用户：
- 本次加载了哪些索引页面（列出路径）
- 加载的总 token 估计
- 是否有 stale 页面被命中（需要先更新再执行）

## 检索边界

### 永远不做的操作
- 不直接 `grep` 或 `find` 搜索整个源码目录
- 不读取 `.work/` 索引未覆盖的源码文件（除非 Step 4 明确需要）
- 不一次性加载超过 10 个索引页面
- 不递归展开超过两层的交叉引用

### 例外：索引未覆盖时的处理
如果任务涉及的文件不在索引覆盖范围内（例如用户选择了部分核心区块，但任务触及了未覆盖的目录），告知用户："此任务涉及 {目录}，但该目录不在核心区块内。是否需要扩展索引覆盖范围（--reconfig）？"
```

---

## references/conventions-template.md

```markdown
# 架构约定

> 此文件在 `--init` 时生成骨架，在 Forge 模式引入新约定时更新。

## 路由约定
- 路由文件位置：
- 路由命名规范：
- 懒加载策略：

## 状态管理约定
- 状态管理方案：
- Store 文件位置：
- Store 命名规范：
- 哪些状态放全局 / 哪些放组件内：

## 组件约定
- 组件目录结构：
- 组件命名规范：
- Props 类型定义位置：
- 样式方案（CSS Modules / Tailwind / styled-components）：

## API 约定
- API 封装层位置：
- 请求拦截器逻辑：
- 错误处理统一方式：
- 类型定义位置：

## 后端约定（Go/Java）
- 分层结构（handler → service → repo）：
- 错误处理模式：
- 中间件位置：
- 数据校验方式：

## 文件命名约定
- 组件文件：
- 页面文件：
- 工具函数文件：
- 类型文件：
```

---

三个 reference 文件 + SKILL.md 都在上面了。后续你在其他地方完善时，需要调整哪部分直接改。