---
name: mbank
description: |
  Memory Bank 工作流，用于复杂项目的结构化开发。
  前提：用户已在项目根目录创建 mbank/ 文件夹和 design.md 设计文档。
  触发场景：(1) 用户输入 /mbank 启动需求澄清和技术栈生成；(2) 用户输入 /init 生成项目规则、实施计划、进度和架构文件；(3) 用户需要基于 mbank/ 文档进行结构化项目开发；(4) 用户提到 memory bank 或 mbank 相关概念。
---

# mbank - Memory Bank 工作流

## 命令

| 命令 | 作用 |
|------|------|
| `/mbank` | 读取 design.md → 澄清需求 → 生成 tech-stack.md |
| `/init` | 生成 CLAUDE.md → 生成 implementation-plan.md → 创建 progress.md 和 architecture.md |

---

## /mbank 流程

### 1. 需求澄清
1. 读取 `mbank/design.md`
2. 分析需求是否完全清晰
3. 如有不明确之处，向用户提问
4. 根据回答可修改/补充 `design.md`
5. 确认后进入下一步

### 2. 生成技术栈
1. 选择**最简单但最健壮**的技术方案
2. 生成 `mbank/tech-stack.md`
3. 向用户展示，等待确认

**技术栈选择原则**:
- 优先成熟稳定的技术
- 避免过度工程化
- 考虑用户技术背景

---

## /init 流程

### 3. 生成项目规则
在项目根目录创建 `CLAUDE.md`，**必须包含**：

```markdown
# 核心工作流 (ALWAYS APPLY)

> [!IMPORTANT]
> **在生成、修改或重构任何代码前，必须：**
>
> 1. **[Always]** 完整阅读 `mbank/architecture.md`
> 2. **[Always]** 完整阅读 `mbank/design.md`
> 3. **[Always]** 完成重大功能后，立即更新 `mbank/architecture.md`
> 4. **[Always]** 任务完成后，询问用户是否更新 `mbank/progress.md`

# 代码规范 (Anti-Monolith)

- **禁止单体大文件**：模块化拆分，单文件不超过 200 行
- **技术栈一致性**：严格遵守 tech-stack.md，禁止引入未授权的第三方包
```

**还应包含**：根据 tech-stack.md 生成的技术栈特定最佳实践。

**关键**：用户必须审查规则后才能继续。

### 4. 生成实施计划
生成 `mbank/implementation-plan.md`：

| 规则 | 说明 |
|------|------|
| 步骤小而具体 | 每步只做一件事 |
| 包含验证测试 | 每步有验证方法 |
| **严禁包含代码** | 只写清晰指令 |
| 根据复杂度调整 | 简单项目可减少步骤 |

**模板**：
```markdown
# [项目名称] - 实施计划

> **版本**: v1.0
> **复杂度评估**: [简单/中等/复杂]

## Phase 0: 项目初始化

### Step 0.1 - [任务名称]
**目标**: [一句话描述]
**操作**: [具体指令，不含代码]
**验证测试**: [如何验证]

## 里程碑检查点
### Milestone 1: [名称]
- [ ] 任务 A
- [ ] 任务 B
```

### 5. 创建进度文件
创建 `mbank/progress.md`：

```markdown
# [项目名称] - 开发进度

## 当前状态
**阶段**: Phase 0
**当前任务**: 等待开始

## 已完成任务
(暂无)

## Errors Encountered
| 错误 | 尝试次数 | 解决方案 |
|------|----------|----------|
```

### 6. 创建架构文件
创建 `mbank/architecture.md`：

```markdown
# [项目名称] - 系统架构

## 当前状态
**版本**: 0.0.1

## 目录结构
(待项目初始化后填充)

## 组件职责
| 文件/组件 | 职责 | 依赖 |
|-----------|------|------|
(待开发时填充)

## 设计决策记录
(待开发时填充)
```

---

## 开发阶段

用户告知执行实施计划后：

1. 阅读 mbank/ 所有文档
2. 执行当前步骤
3. 等待用户验证
4. 通过后更新 progress.md 和 architecture.md
5. 进入下一步

**关键规则**:
- **逐步验证**: 验证通过前不开始下一步
- **2-Action Rule**: 每 2 次搜索后将发现写入文档
- **3-Strike Protocol**: 同一错误尝试 3 次后换方法或询问用户

---

## 文件结构

```
项目根目录/
├── CLAUDE.md              # 项目规则（/init 生成）
└── mbank/
    ├── design.md          # 设计文档（用户创建）
    ├── tech-stack.md      # 技术栈（/mbank 生成）
    ├── implementation-plan.md  # 实施计划（/init 生成）
    ├── progress.md        # 进度追踪（/init 生成）
    └── architecture.md    # 架构文档（/init 生成）
```
