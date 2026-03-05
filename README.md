# mbank - Memory Bank 工作流

一个用于复杂项目结构化开发的 Cursor / Claude Code Skill。通过文档驱动的方式，帮助 AI 理解项目上下文、遵循开发规范、追踪进度。

## 安装

将 `SKILL.md` 放入 skills 目录：

```bash
# Cursor
cp SKILL.md ~/.cursor/skills/mbank/SKILL.md

# Claude Code
cp SKILL.md ~/.claude/skills/mbank/SKILL.md
```

## 快速开始

### 1. 准备项目

在你的项目根目录创建 `mbank/` 文件夹和设计文档：

```bash
mkdir mbank
touch mbank/design.md
```

### 2. 编写设计文档

在 `mbank/design.md` 中描述你的项目需求，例如：

```markdown
# 我的项目

## 目标
构建一个 TODO 应用

## 功能需求
- 添加任务
- 删除任务
- 标记完成

## 技术偏好
- 前端：React
- 后端：无（本地存储）
```

### 3. 启动工作流

```
/mbank
```

Claude 会：
1. 读取你的设计文档
2. 提问澄清不明确的需求
3. 生成技术栈建议 (`mbank/tech-stack.md`)

### 4. 初始化项目

确认技术栈后执行：

```
/init
```

Claude 会生成：
- `CLAUDE.md` - 项目规则（在根目录）
- `mbank/implementation-plan.md` - 实施计划
- `mbank/progress.md` - 进度追踪
- `mbank/architecture.md` - 架构文档
- `mbank/quickref.md` - 开发者速查卡

### 5. 开始开发

告诉 Claude 执行实施计划，它会逐步实现功能并更新文档。

## 文件结构

```
你的项目/
├── CLAUDE.md              # 项目规则（/init 生成，始终加载）
├── .cursor/
│   └── rules/             # 文件级上下文规则（开发阶段按需生成）
│       ├── downloader.mdc # 示例：编辑 downloader.py 时自动注入
│       └── ...
└── mbank/
    ├── design.md              # 设计文档（你创建）
    ├── tech-stack.md          # 技术栈（/mbank 生成）
    ├── implementation-plan.md # 实施计划（/init 生成）
    ├── progress.md            # 进度追踪（/init 生成）
    ├── architecture.md        # 完整架构（/init 生成）
    └── quickref.md            # 开发者速查卡（/init 生成）
```

## 上下文加载层级

mbank 使用分层上下文管理，既保证 AI 每次对话都掌握全局，又避免上下文膨胀：

| 层级 | 文件 | 加载时机 | 内容 |
|------|------|----------|------|
| L1 常驻 | `CLAUDE.md` | 每次对话 | 项目规则、必读文件列表 |
| L2 常驻 | `mbank/quickref.md` | 每次对话 | 关键路径、活跃流程、踩坑记录 |
| L3 常驻 | `mbank/architecture.md` | 每次对话 | 完整架构、组件职责、设计决策 |
| L4 常驻 | `mbank/design.md` | 每次对话 | 完整设计文档 |
| L5 常驻 | `mbank/tech-stack.md` | 每次对话 | 技术栈约束 |
| L6 常驻（部分）| `mbank/progress.md` | 每次对话，仅读顶部 `## 当前状态` | 当前阶段、最近任务 |
| L7 按需 | `.cursor/rules/*.mdc` | 编辑匹配文件时 | 模块级实现细节、约束、陷阱 |
| L8 按需 | `mbank/progress.md` 全文 | 需要查历史/错误记录时 | 完整进度历史 |

## 核心特性

- **需求澄清**: 自动分析设计文档，提问不明确之处
- **技术栈选择**: 推荐最简单但最健壮的技术方案
- **强制规则**: 确保 AI 在写代码前阅读架构和设计文档
- **Anti-Monolith**: 强制模块化，单文件不超过 200 行
- **逐步验证**: 每步完成后验证，通过才继续
- **进度追踪**: 自动记录完成的任务和遇到的错误
- **开发者速查卡** (`quickref.md`): 高频查找信息的速查卡，避免每次重读所有文档
- **按需上下文规则** (`.cursor/rules/`): 编辑特定文件时自动注入模块级上下文
- **分层上下文管理**: L1-L8 分层加载，兼顾全局视野和上下文效率

## 许可证

MIT
