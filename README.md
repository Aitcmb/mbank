# mbank

面向 AI Agent（Claude Code、Codex、Cursor）的结构化项目开发 Skill。

`mbank` 帮助 Agent 把模糊的产品想法转化为有纪律的实施工作流，通过分层项目文档代替临时聊天记忆来保持上下文一致。

## 为什么需要 mbank

大多数 Agent 工作流擅长写代码，但在以下方面很弱：
- 把模糊想法转化为可用的设计简报
- 在长对话中保持项目上下文
- 保持架构、进度和实施文档同步

`mbank` 通过文档驱动的工作流解决这些问题：

```text
/discover → /mbank → /scaffold → 开发 → /check → /archive
```

## 核心功能

- `/discover`
  将模糊想法发散并收敛为 `mbank/discovery.md` 和 `mbank/design.md`
- `/mbank`
  将 `mbank/design.md` 转化为澄清后的技术方案，生成 `mbank/tech-stack.md`
- `/scaffold`
  生成项目运作文档：`AGENTS.md`、`implementation-plan.md`、`progress.md`、`architecture.md`、`quickref.md`
- `/check`
  执行文档一致性验证和可选的回归测试
- `/archive`
  里程碑封版，归档已完成进度并压缩活跃文档

## 安装

```bash
# Claude Code
git clone https://github.com/Aitcmb/mbank.git ~/.claude/skills/mbank

# Codex / OpenAI Agents
git clone https://github.com/Aitcmb/mbank.git ~/.codex/skills/mbank

# Cursor
git clone https://github.com/Aitcmb/mbank.git ~/.cursor/skills/mbank
```

## 快速开始

### 1. 创建项目文件夹

```bash
mkdir mbank
```

### 2. 从发散开始

如果项目想法还很模糊：

```text
/discover
```

产出：
- `mbank/discovery.md`
- `mbank/design.md`

### 3. 确认技术方向

```text
/mbank
```

这一步会：
- 读取 `mbank/design.md`
- 补齐缺失的约束条件
- 评估项目复杂度
- 生成 `mbank/tech-stack.md`

### 4. 生成项目骨架

```text
/scaffold
```

产出：
- `AGENTS.md`
- `CLAUDE.md`（可选兼容文件）
- `mbank/implementation-plan.md`
- `mbank/progress.md`
- `mbank/architecture.md`
- `mbank/quickref.md`
- `mbank/context/`

### 5. 开发、验证、归档

```text
/check
/archive
```

## 核心文件结构

```text
你的项目/
├── AGENTS.md
├── CLAUDE.md
├── .claude/
│   └── rules/
└── mbank/
    ├── discovery.md
    ├── design.md
    ├── tech-stack.md
    ├── quickref.md
    ├── architecture.md
    ├── implementation-plan.md
    ├── progress.md
    ├── context/
    └── archive/
```

## 上下文模型

`mbank` 采用渐进式披露的分层上下文，而非一次性全量加载：

- `quickref.md`
  冷启动唯一入口：当前状态、关键路径索引、踩坑速览、深入阅读指引
- `AGENTS.md`
  项目主规则文件
- `.claude/rules/`
  全局硬约束
- `architecture.md`
  架构与组件职责
- `design.md`
  正式的产品/设计定义
- `discovery.md`
  前期发散的推理与范围取舍
- `progress.md`
  当前进度与活跃状态
- `context/`
  模块级深度上下文，仅在触及对应模块时加载

## 仓库文件说明

- `SKILL.md`
  主 Skill 指令（触发 + 约束 + 执行骨架）
- `defaults.md`
  个人偏好模板
- `references/`
  按需加载的详细流程和模板
- `CHANGELOG.md`
  版本历史

## 许可证

MIT
