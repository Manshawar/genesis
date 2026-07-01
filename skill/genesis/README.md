# Genesis

前后端分离项目的工程开发助手。

## 功能

- **精读项目**：首次使用 `--init` 生成 `.genesis/` 索引
- **精准检索**：写需求、修 bug、写测试时按混合协议检索上下文
- **双模式支持**：
  - **Forge**：新建模块、改架构、引入新约定
  - **Quick**：在现有模块和约定内修改

## 技术栈

前端：React / Vue
后端：Go / Java

## 触发词

开始工作、修 bug、加功能、写测试、重构、读项目、熟悉项目、帮我理解这个项目

## 目录结构

```
skill/genesis/skill/
├── SKILL.md              # Skill 定义文件
└── references/           # 参考文档
    ├── conventions-template.md
    ├── init-protocol.md
    └── retrieval-protocol.md
```

## 使用方式

```bash
# 首次使用：精读项目
Genesis --init

# 常规工作
Genesis <任务描述>

# Forge 模式（新建模块/改架构）
Genesis --mode forge <任务描述>

# Quick 模式（在现有模块内修改）
Genesis --mode quick <任务描述>

# 重新配置
Genesis --reconfig

# 仅预览计划
Genesis --dry-run <任务描述>
```