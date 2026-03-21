# mbank

`mbank` 是一个面向 Claude Code / Codex / Cursor 类 agent 的结构化项目开发 skill。它把“前期发散、设计收敛、技术栈确认、项目骨架生成、验证、归档”拆成一组明确命令，核心是文档驱动和分层上下文管理。

## 安装

```bash
# Claude Code
git clone https://github.com/Aitcmb/mbank.git ~/.claude/skills/mbank

# Codex / OpenAI Agents
git clone https://github.com/Aitcmb/mbank.git ~/.codex/skills/mbank

# Cursor
git clone https://github.com/Aitcmb/mbank.git ~/.cursor/skills/mbank
```

## 工作流

推荐顺序：

```text
/discover -> /mbank -> /scaffold
```

- `/discover`
  从模糊想法或粗糙需求出发，完成痛点澄清、问题重构、范围分叉，并生成 `mbank/discovery.md` 与 `mbank/design.md`
- `/mbank`
  读取 `mbank/design.md`，做正式澄清、复杂度评估，生成 `mbank/tech-stack.md`
- `/scaffold`
  根据复杂度和 `mbank/tech-stack.md` 生成 `AGENTS.md`、可选 `CLAUDE.md`、`implementation-plan`、`progress`、`architecture`、`quickref`
- `/check`
  用户显式触发的完整验证：执行验证命令并检查文档一致性
- `/archive`
  用户显式触发的里程碑封版：归档已完成任务、重置当前状态、生成阶段总结

如果已经有成熟的 `mbank/design.md`，可直接从 `/mbank` 开始。

## 快速开始

### 1. 准备项目目录

在项目根目录创建 `mbank/`：

```bash
mkdir mbank
```

### 2. 前期发散

当想法还模糊时，执行：

```text
/discover
```

该阶段会沉淀两份文档：
- `mbank/discovery.md`
- `mbank/design.md`

模板位于：
- `references/discovery-template.md`
- `references/design-template.md`

### 3. 确认技术方案

```text
/mbank
```

该阶段会：
- 读取 `mbank/design.md`
- 按项目类型补齐关键约束
- 评估复杂度
- 生成 `mbank/tech-stack.md`

### 4. 生成项目骨架

```text
/scaffold
```

该阶段会生成：
- `AGENTS.md`
- `CLAUDE.md`（可选兼容）
- `mbank/implementation-plan.md`
- `mbank/progress.md`
- `mbank/architecture.md`
- `mbank/quickref.md`
- `mbank/context/`

模板位于：
- `references/scaffold-templates.md`

### 5. 开发、验证与归档

```text
/check
/archive
```

- `/check` 只在用户显式触发时做完整验证
- `/archive` 只在用户显式触发时做一次里程碑封版

## 核心文件

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

## 上下文层级

- `AGENTS.md`
  项目主规则文件，常驻
- `CLAUDE.md`
  可选兼容入口
- `.claude/rules/`
  全局硬规则，仅放跨模块不变约束
- `mbank/quickref.md`
  常驻速查层
- `mbank/architecture.md`
  常驻架构层
- `mbank/design.md`
  常驻设计层
- `mbank/discovery.md`
  按需读取的前期发散层
- `mbank/progress.md`
  默认只读 `## 当前状态`
- `mbank/context/`
  模块深度上下文，按需加载

## 仓库内容

- `SKILL.md`
  主 skill 定义
- `defaults.md`
  个人偏好示例
- `references/`
  仅在需要时加载的模板文件

## 许可证

MIT
