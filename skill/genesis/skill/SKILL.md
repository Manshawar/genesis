---
name: genesis
description: "前后端分离项目的工程开发助手。精读项目生成 .genesis/ 索引，后续写需求、修 bug、写测试时按混合协议检索精准上下文。支持 Forge（新建模块/改架构）和 Quick（照葫芦画瓢）两种模式。前端 React/Vue，后端 Go/Java。触发：开始工作、修 bug、加功能、写测试、重构、读项目、熟悉项目、帮我理解这个项目。"
argument-hint: [--mode forge|quick] [--init] [--reconfig] [--dry-run] [task]
---

# Genesis

IRON LAW: 每次工作前必须先读 `.genesis/_index.md`。不知道项目里有什么就动手，是所有 bug 的源头。

## $ARGUMENTS

| 参数 | 说明 | 默认 |
|------|------|------|
| `<task>` | 任务描述（修 bug、加功能、写测试、读项目） | 必填 |
| `--mode forge` | 涉及新建模块/改架构/引入新约定 | 自动判断 |
| `--mode quick` | 在现有模块和约定内修改 | 自动判断 |
| `--init` | 首次精读项目，生成 `.genesis/` | - |
| `--reconfig` | 重新选择核心区块和技术栈 | - |
| `--dry-run` | 只输出计划，不改代码 | false |

模式自动判断：任务涉及新建路由模块、引入新依赖、改变状态管理方式 → Forge。其他 → Quick。

## 工作流

```
Genesis Progress:

- [ ] Step 0: 环境检查
  - [ ] .genesis/_config.md 存在？否 → 提示执行 --init
  - [ ] 运行 git diff --stat，标记 [stale] 的索引页面
- [ ] Step 1: 读索引 ⚠️ REQUIRED
  - [ ] 读 .genesis/_index.md（全局地图）
  - [ ] 读 .genesis/_config.md（核心区块 + 技术栈）
  - [ ] 读 .genesis/_conventions.md（架构约定）
  - [ ] 判断模式并告知用户
- [ ] Step 2: 检索上下文 ⚠️ REQUIRED
  - [ ] 必查层：.genesis/shared/*.md + .genesis/patterns/*.md（自动加载）
  - [ ] 前端按需层：根据任务关键字匹配 .genesis/frontend/*.md
  - [ ] 后端按需层：根据任务关键字匹配 .genesis/backend/*.md
  - [ ] 沿 _index.md 中的链接定向深入
  - [ ] 汇总：告知用户引用了哪些模块/约定
- [ ] Step 3: 制定变更计划
  - Forge: 列出所有受影响模块和新约定，逐项确认
  - Quick: 列出变更清单，一次确认
- [ ] Step 4: 执行
  - [ ] 读写代码文件
  - [ ] 同步更新 .genesis/ 索引（受影响页面标记为已更新）
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
5. 生成 `.genesis/` 索引并保存配置
6. 告知用户索引覆盖范围

## .genesis/ 索引结构

```
.genesis/
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

每次工作前执行 `git diff --stat`。变更文件对应的 `.genesis/` 索引页面标记为 `[stale]`。若 stale 页面被本次任务命中，先更新索引再执行。

## 反模式

- **不读索引就动手**：不知道项目结构就开始写代码
- **Quick 模式引入新约定**：照葫芦画瓢时偷偷改变了架构约定
- **全量扫描源码**：不读 `.genesis/` 索引，直接 `grep` 整个项目
- **索引不更新**：改完代码不更新 `.genesis/`，下次工作索引就是错的
- **忽略 _config.md**：不知道核心区块和技术栈就开始读代码
- **忽略 _conventions.md**：团队有约定但 AI 不读，写出风格不一致的代码
- **跳过 Step 0**：不检查 stale 标记就开始工作，用过期索引做决策