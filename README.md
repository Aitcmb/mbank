# mbank 工作流

一个用于结构化项目开发的 Cursor / Claude Code Skill。通过文档驱动 + 三层上下文管理，帮助 AI 理解项目上下文、遵循开发规范、追踪进度。

## 安装

```bash
# Claude Code
git clone https://github.com/Aitcmb/mbank.git ~/.claude/skills/mbank

# Cursor
git clone https://github.com/Aitcmb/mbank.git ~/.cursor/skills/mbank
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

### 3. (可选) 配置个人偏好

编辑 `~/.claude/skills/mbank/defaults.md`，填写你的技术栈偏好和通用规则。配置后所有项目都会参考此偏好。

### 4. 启动工作流

```
/mbank
```

Claude 会：
1. 读取个人偏好（如有）
2. 读取设计文档，使用场景化清单澄清需求（APP/游戏/工具）
3. 评估项目复杂度（轻量/标准/复杂）
4. 生成技术栈建议 (`mbank/tech-stack.md`)

### 5. 初始化项目

确认技术栈后执行：

```
/init
```

根据复杂度模式生成对应文件：
- **轻量模式**（小工具）: `CLAUDE.md` + `progress.md`
- **标准模式**（APP）: 全套文件 + `mbank/context/` 目录
- **复杂模式**（大型游戏）: 全套 + 模块拆分 spec

### 6. 开始开发

告诉 Claude 执行实施计划，它会逐步实现功能并更新文档。

### 7. 验证与归档

```
/check    # 执行验证命令 + 文档一致性检查
/archive  # 归档当前版本，重置进度
```

## 命令

| 命令 | 作用 |
|------|------|
| `/mbank` | 读取偏好 → 读取 design.md → 场景化澄清 → 复杂度评估 → 生成 tech-stack.md |
| `/init` | 根据复杂度模式生成对应文件集 |
| `/check` | 执行已完成步骤的验证命令 + 文档一致性检查 |
| `/archive` | 归档当前版本 → 重置进度 → 生成里程碑总结 |

## 文件结构

```
你的项目/
├── CLAUDE.md                    # 项目规则 + 文档写入决策规则（始终加载）
├── .claude/
│   └── rules/                   # 全局硬规则（自动加载，仅放跨模块约束）
│       └── global-constraints.md
└── mbank/
    ├── design.md                # 设计文档（你创建）
    ├── tech-stack.md            # 技术栈（/mbank 生成）
    ├── quickref.md              # 开发者速查卡 + 模块索引（/init 生成）
    ├── architecture.md          # 完整架构（/init 生成）[标准+]
    ├── implementation-plan.md   # 实施计划（/init 生成）[标准+]
    ├── progress.md              # 进度追踪（/init 生成）
    ├── context/                 # 模块深度上下文（开发阶段按需生成）
    │   ├── downloader.md
    │   └── ...
    └── archive/                 # 里程碑归档（/archive 生成）
        └── v0.1.md
```

## 三层上下文架构

mbank 使用三层分离的上下文管理，兼顾全局视野和上下文效率：

| 层 | 位置 | 加载方式 | 放什么 | 不放什么 |
|---|------|---------|--------|---------|
| **硬规则** | `.claude/rules/` | 自动全加载 | 跨模块不变约束（禁令/风格） | 模块实现细节 |
| **索引** | `quickref.md` | 每次必读 | 关键路径 + 模块→context 映射 + 踩坑摘要 | 详细踩坑 |
| **深度上下文** | `mbank/context/` | 按需加载 | 模块职责、流程、约束、踩坑详情 | 全局规则 |

**写入速查**: `.claude/rules/` 放"不许做什么"，`mbank/context/` 放"该怎么做"

### 上下文加载层级

| 层级 | 文件 | 加载时机 | 内容 |
|------|------|----------|------|
| L1 常驻 | `CLAUDE.md` | 每次对话 | 项目规则、文档写入决策规则 |
| L2 自动 | `.claude/rules/*.md` | 会话启动全加载 | 跨模块硬规则（< 30 行） |
| L3 常驻 | `mbank/quickref.md` | 每次对话必读 | 关键路径索引 + context 映射 |
| L4 常驻 | `mbank/architecture.md` | 每次对话 | 完整架构 |
| L5 常驻 | `mbank/design.md` | 每次对话 | 设计文档 |
| L6 常驻 | `mbank/tech-stack.md` | 每次对话 | 技术栈约束 |
| L7 常驻（部分） | `mbank/progress.md` | 仅读 `## 当前状态` | 当前进度 |
| L8 **按需** | `mbank/context/*.md` | 触及模块时 | 模块深度上下文 |
| L9 按需 | `mbank/progress.md` 全文 | 查历史时 | 完整进度历史 |
| L10 按需 | `mbank/archive/*.md` | 查历史版本时 | 里程碑归档 |

## 核心特性

- **场景化需求澄清**: APP/游戏/工具各有专用澄清清单
- **复杂度自适应**: 轻量/标准/复杂三种模式，小工具不需要 6 个文档
- **个人偏好持久化**: `defaults.md` 跨项目复用技术栈偏好
- **三层上下文分离**: 硬规则自动加载 + 索引常驻 + 模块按需深入
- **可执行验证**: `/check` 命令运行验证命令并检查文档一致性
- **版本归档**: `/archive` 命令归档里程碑，保持进度文件清洁
- **Anti-Monolith**: 强制模块化，单文件不超过 200 行
- **逐步验证**: 验证通过前不开始下一步
- **3-Strike Protocol**: 同一错误尝试 3 次后换方法或询问用户
- **文档熵治理**: 每 3 个 step 自动检查文档与代码的一致性

## 许可证

MIT
