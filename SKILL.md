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
| `/init` | 生成 CLAUDE.md → implementation-plan.md → progress.md → architecture.md → quickref.md |

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
> 1. **[Always]** 完整阅读 `mbank/quickref.md`
> 2. **[Always]** 完整阅读 `mbank/architecture.md`
> 3. **[Always]** 完整阅读 `mbank/design.md`
> 4. **[Always]** 完整阅读 `mbank/tech-stack.md`
> 5. **[Always]** 阅读 `mbank/progress.md` 的 `## 当前状态` 区块（仅顶部，无需全文）
> 6. **[Always]** 完成重大功能后，立即更新 `mbank/architecture.md` 和 `mbank/quickref.md`
> 7. **[Always]** 完成重大功能后，为新模块生成或更新 `.cursor/rules/` 上下文规则
> 8. **[Always]** 任务完成后，询问用户是否更新 `mbank/progress.md`

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
<!-- 此区块是常驻上下文，每次对话都会读取，保持 10 行以内 -->
**阶段**: Phase 0
**当前任务**: 等待开始
**最后更新**: [日期]

## 已完成任务
(暂无)

## Errors Encountered
| 错误 | 尝试次数 | 解决方案 |
|------|----------|----------|
```

**关键约定**：`## 当前状态` 区块始终在文件最顶部，保持 10 行以内。每次更新 progress.md 时同步刷新此区块，使其反映最新状态。

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

### 7. 创建快速参考
创建 `mbank/quickref.md`——**开发者速查卡**，每次对话首先阅读：

```markdown
# [项目名称] - 快速参考

> 本文件是开发者速查卡，记录高频查找的信息。
> 每次完成重大功能后同步更新。

## 关键路径
| 功能 | 入口 | 核心文件 |
|------|------|----------|
(待开发时填充)

## 活跃工作流
(记录当前系统的核心操作流程，用简短的步骤链表示)

## 踩坑记录
| 坑 | 原因 | 正确做法 |
|----|------|----------|
(开发过程中遇到的关键陷阱，防止重复踩坑)
```

**quickref.md 的定位**：
- `architecture.md` = 完整架构文档（组件职责、数据流、设计决策）
- `quickref.md` = 开发者速查卡（关键路径、活跃流程、踩坑记录）
- quickref 只放"打开项目第一分钟需要知道的事"，不重复 architecture 的内容

---

## 开发阶段

用户告知执行实施计划后：

1. 阅读 mbank/ 所有文档
2. 执行当前步骤
3. 等待用户验证
4. 通过后更新 progress.md 和 architecture.md
5. **同步更新 quickref.md**（关键路径、活跃工作流、新踩坑记录）
6. **为完成的模块生成/更新 `.cursor/rules/` 上下文规则**
7. 进入下一步

**关键规则**:
- **逐步验证**: 验证通过前不开始下一步
- **2-Action Rule**: 每 2 次搜索后将发现写入文档
- **3-Strike Protocol**: 同一错误尝试 3 次后换方法或询问用户

---

## .cursor/rules/ 上下文规则生成

完成一个重大模块后，在 `.cursor/rules/` 下创建对应的 `.mdc` 文件。
规则文件是**按需加载**的——只在编辑匹配文件时自动注入，不占用全局上下文。

### 何时生成
- 完成一个新模块/功能后
- 遇到重大踩坑后（防止下次编辑同一文件时重蹈覆辙）
- 模块涉及外部系统交互（API、页面结构、选择器等易变信息）

### 文件格式
```markdown
---
globs: [匹配的文件 glob 模式]
---
[模块名] 上下文：

- **职责**: 一句话描述
- **核心流程**: 步骤链
- **关键约束**: 必须遵守的规则
- **踩坑提醒**: 已知陷阱
```

### 命名约定
- 文件名 = 模块名或功能域名，如 `downloader.mdc`、`hero-analysis.mdc`
- globs 匹配对应的源文件，如 `scripts/downloader.py`、`scripts/hero_*.py`

### 示例
```markdown
---
globs: scripts/downloader.py
---
downloader 上下文：

- **职责**: 批量下载 DataWind 看板数据
- **核心流程**: 启动浏览器 → 遍历 URL → 打开菜单 → 导出 CSV → 保存
- **关键约束**: 必须用 launch_persistent_context 复用登录会话
- **踩坑提醒**: 棋手面板的"收起工具栏"按钮(arco-icon-N-Down1)不要点，会导致工具栏消失
```

---

## 文件结构

```
项目根目录/
├── CLAUDE.md              # 项目规则（/init 生成，始终加载）
├── .cursor/
│   └── rules/             # 文件级上下文规则（开发阶段按需生成）
│       ├── downloader.mdc # 示例：编辑 downloader.py 时自动注入
│       └── ...
└── mbank/
    ├── design.md              # 设计文档（用户创建）
    ├── tech-stack.md          # 技术栈（/mbank 生成）
    ├── implementation-plan.md # 实施计划（/init 生成）
    ├── progress.md            # 进度追踪（/init 生成）
    ├── architecture.md        # 完整架构（/init 生成）
    └── quickref.md            # 开发者速查卡（/init 生成）
```

### 上下文加载层级

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
