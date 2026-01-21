# mbank - Memory Bank 工作流

一个用于复杂项目结构化开发的 Claude Code Skill。通过文档驱动的方式，帮助 AI 理解项目上下文、遵循开发规范、追踪进度。

## 安装

将 `mbank.skill` 文件或整个 `mbank/` 文件夹放入 Claude Code 的 skills 目录：

```bash
# 方式 1: 复制文件夹
cp -r mbank ~/.claude/skills/

# 方式 2: 使用 .skill 包
# 将 mbank.skill 放入 ~/.claude/skills/
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

在 Claude Code 中执行：

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

### 5. 开始开发

告诉 Claude 执行实施计划，它会逐步实现功能并更新文档。

## 文件结构

```
你的项目/
├── CLAUDE.md                    # 项目规则
└── mbank/
    ├── design.md                # 设计文档（你创建）
    ├── tech-stack.md            # 技术栈（/mbank 生成）
    ├── implementation-plan.md   # 实施计划（/init 生成）
    ├── progress.md              # 进度追踪（/init 生成）
    └── architecture.md          # 架构文档（/init 生成）
```

## 核心特性

- **需求澄清**: 自动分析设计文档，提问不明确之处
- **技术栈选择**: 推荐最简单但最健壮的技术方案
- **强制规则**: 确保 AI 在写代码前阅读架构和设计文档
- **Anti-Monolith**: 强制模块化，单文件不超过 200 行
- **逐步验证**: 每步完成后验证，通过才继续
- **进度追踪**: 自动记录完成的任务和遇到的错误

## 许可证

MIT
