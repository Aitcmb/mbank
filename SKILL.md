---
name: mbank
description: |
  mbank 工作流，用于结构化项目开发与前期发散。适用于以下场景：(1) 用户输入 `/discover`，从模糊想法或粗糙需求生成 `mbank/discovery.md` 与 `mbank/design.md`；(2) 用户输入 `/mbank`，基于 `mbank/design.md` 做正式澄清、复杂度评估并生成 `mbank/tech-stack.md`；(3) 用户输入 `/scaffold`，生成 `AGENTS.md`、`implementation-plan`、`progress`、`architecture`、`quickref` 等项目骨架文档；(4) 用户输入 `/check` 做完整验证；(5) 用户输入 `/archive` 做里程碑封版；(6) 用户需要基于 `mbank/` 文档进行结构化项目开发；(7) 用户提到 mbank 相关概念。
---

# mbank 工作流

按需读取引用文件：
- 执行 `/discover` 时，读取 `references/discovery-template.md` 与 `references/design-template.md`
- 执行 `/scaffold` 时，读取 `references/scaffold-templates.md`

## 命令

| 命令 | 作用 |
|------|------|
| `/discover` | 从想法或粗糙需求出发，完成痛点澄清、问题重构、范围分叉，并生成 `mbank/discovery.md` 与 `mbank/design.md` |
| `/mbank` | 读取 `mbank/design.md`，做正式澄清、复杂度评估，生成 `mbank/tech-stack.md` |
| `/scaffold` | 根据复杂度和 `mbank/tech-stack.md` 生成 `AGENTS.md`、可选 `CLAUDE.md`、`implementation-plan`、`progress`、`architecture`、`quickref` |
| `/check` | 用户显式触发的完整验证：执行验证命令并检查文档一致性 |
| `/smoke` | 启动桌面应用冒烟测试：启动应用 → 检查阻断性问题 → 报告 PASS/FAIL/PARTIAL |
| `/archive` | 用户显式触发的里程碑封版：归档已完成任务、重置当前状态、生成阶段总结 |

## 推荐顺序

- 想法还模糊：`/discover` → `/mbank` → `/scaffold`
- 已有成熟 `mbank/design.md`：`/mbank` → `/scaffold`
- 已完成骨架初始化：直接按实施计划进入开发

---

## /discover

用于把“想做什么”收敛成可执行的 `mbank/design.md`。若已存在 `mbank/design.md` 但仍只是功能清单，也先执行 `/discover`。

### 核心步骤

1. 读取已有材料
- 若 `mbank/design.md` 已成熟，可建议直接进入 `/mbank`
- 若缺少用户、场景、约束或范围边界，继续发散

2. 痛点澄清
- 必须明确：谁遇到问题、在哪个场景、当前怎么做、现有方案为什么不够、频率如何、不解决的成本是什么
- 优先要求具体例子，不接受纯抽象愿景

3. 问题重构
- 用一句话总结原始想法
- 用一句话改写成真正要解决的问题
- 提炼 3-6 条可确认的产品前提

4. 隐含能力提取
- 区分：核心能力、支撑能力、暂不纳入范围
- 只基于用户场景提取，不凭空脑补

5. 范围分叉
- 必须给出 3 条路线：最小切口版、平衡版、愿景版
- 每条都写清：解决什么、不解决什么、复杂度、风险
- 默认推荐最小切口版，除非有明确理由

6. 收敛
- 生成 `mbank/discovery.md`
- 生成 `mbank/design.md`
- 模板见 `references/discovery-template.md` 与 `references/design-template.md`
- 只有在 `mbank/design.md` 草稿完成后，才能进入 `/mbank`

### 结束条件

- 用户、场景、痛点明确
- 产品前提已确认
- 已比较至少 3 条路线
- 已选定推荐路线
- `mbank/design.md` 草稿已生成

---

## /mbank

用于把 `mbank/design.md` 转成可执行的技术方案。

### 输入守卫

- 若不存在 `mbank/design.md`，先执行 `/discover`
- 若 `mbank/design.md` 仍缺少用户、场景、约束或范围边界，先执行 `/discover`
- 如存在 `mbank/discovery.md`，仅在需要理解前提或取舍时按需读取

### 核心步骤

1. 读取个人偏好
- 检查 `~/.claude/skills/mbank/defaults.md`
- 如存在，作为技术栈选择参考

2. 正式澄清
- 判断项目类型：APP / 游戏 / 工具
- 用对应清单补齐缺失信息
- 需要时修改 `mbank/design.md`

APP 类至少澄清：
- 目标平台
- 离线需求
- 认证方式
- 数据存储方案
- 实时功能需求

游戏类至少澄清：
- 核心循环
- 目标平台和输入方式
- 2D/3D 与美术方向
- 联网需求
- 引擎偏好
- 音频需求

工具类至少澄清：
- 交互形式
- 输入输出格式
- 是否跨平台
- 目标用户

3. 复杂度评估
- 指标：文件/模块数量、前后端分离、外部 API/数据库、状态管理、TDD 适用性

| 模式 | 判断条件 | `/scaffold` 生成结果 | TDD |
|------|---------|----------------------|-----|
| 轻量 | < 3 个文件/模块，无后端，单一功能 | `AGENTS.md` + `mbank/design.md` + `mbank/progress.md` | 可选 |
| 标准 | 多模块，前后端分离，需要架构设计 | 全套文件 + `mbank/context/` | 必须 |
| 复杂 | 大型游戏/多系统交互 | 全套 + 模块拆分 spec + 子系统 context | 必须 |

4. 生成技术栈
- 生成 `mbank/tech-stack.md`
- 原则：简单、健壮、成熟、避免过度工程化
- 向用户展示并等待确认

---

## /scaffold

用于在技术栈和复杂度确认后生成项目骨架文档。

### 生成内容

- `AGENTS.md`
- 可选 `CLAUDE.md`
- `mbank/implementation-plan.md`
- `mbank/progress.md`
- `mbank/architecture.md`
- `mbank/quickref.md`
- `mbank/context/`

具体模板见 `references/scaffold-templates.md`。

### 开发阶段规则

1. 先读：`quickref.md` 的 `## 当前焦点` → `progress.md` 的 `## 当前状态` → 焦点指定的 `mbank/context/` 文件
2. 其他文件（architecture、design、tech-stack）按需加载，不要一次性全读
3. 标准/复杂模式严格遵循 TDD：Red → Green → Refactor
4. 验证通过前不进入下一步
5. **每完成 1 个 Step**：更新 `progress.md` 当前状态 + `quickref.md` 当前焦点（Phase/Step/需要读取）
6. **每完成 3 个 Step**：全面同步——quickref 关键路径表、architecture 组件表、context/ 覆盖率检查
7. 需要时新增或更新 `mbank/context/{模块名}.md`

### 子代理委托（标准/复杂模式）

默认顺序执行。当满足以下**任一**条件时，切换为并行子代理模式：
- 当前 Phase 包含 2+ 个互不依赖的 Step（无共享文件修改）
- 用户显式要求并行执行

**委托规则**：
1. 主上下文作为调度器：读取 quickref + architecture，提取待执行 Step 的完整文本
2. 每个 Step 派发一个子代理，prompt 中直接包含：Step 全文、相关 context 内容、tech-stack 约束（子代理不自行读 mbank/ 文档）
3. 子代理完成后报告状态：**DONE** / **DONE_WITH_CONCERNS** / **NEEDS_CONTEXT** / **BLOCKED**
4. 主上下文负责：独立验证结果、检查文件冲突、更新 progress/architecture/quickref
5. 同一文件只能被一个子代理修改——若有冲突则改为顺序执行

**不使用子代理的情况**：
- 轻量模式（<3 文件/模块）
- Steps 之间有严格顺序依赖
- 用户显式要求顺序执行

---

## /check

仅在用户显式触发时执行完整验证：

1. 读取 `mbank/implementation-plan.md`
2. 找到所有已完成步骤的验证命令
3. 依次执行验证命令
4. 汇报通过/失败结果
5. 对失败项提出修复建议
6. 执行完整文档一致性检查

---

## /smoke

轻量级应用冒烟测试。与 `/check` 不同，`/smoke` 不执行实施计划中的验证命令，而是从用户视角验证应用能否正常启动和基本交互。

### 输入

读取 `mbank/smoke-test.md` 获取：启动命令、成功标志、核心冒烟用例、已知可忽略问题。

如不存在 `mbank/smoke-test.md`，提示用户先用 `/scaffold` 生成，或参考 `references/app-validation-template.md` 手动创建。

### 流程

1. 读取 `mbank/smoke-test.md`
2. 执行启动命令，检查进程是否存活（无崩溃、无阻断性错误）
3. 如有 Playwright MCP 或其他自动化工具，执行冒烟用例
4. 如无自动化工具，列出需用户手动验证的检查点
5. 汇报结果：
   - **PASS**：应用启动正常，核心路径可用
   - **FAIL**：列出阻断性问题 + 修复建议
   - **PARTIAL**：应用启动但部分功能异常

**Iron Law**：必须有运行结果的新鲜证据，不得仅基于代码阅读推测应用状态。

### 参考

不同项目类型的验证模式见 `references/app-validation-template.md`（三级验证：L0 编译 → L1 冒烟 → L2 完整）。

---

## /archive

仅在用户显式触发时执行。作用是做一次里程碑封版，而不是替代 git。

### 执行内容

1. 确认当前里程碑名称和版本号
2. 将 `progress.md` 的已完成任务移入 `mbank/archive/v{版本号}.md`
3. 重置 `progress.md` 的 `## 当前状态`
4. 更新 `architecture.md` 的版本号或阶段状态
5. 检查 `quickref.md` 完整性
6. 生成阶段总结：已完成功能、已知问题、下阶段方向

### 适用场景

- MVP 完成
- 一个 milestone 完成
- 准备进入下一大阶段

---

## 上下文层级

### 每次对话必读（2 个文件）
- `AGENTS.md`：项目规则 + 会话恢复协议
- `mbank/quickref.md`：上下文路由层（当前焦点 → 告诉你下一步读什么）

### 按需加载（quickref 指引）
- `mbank/progress.md`：默认只读 `## 当前状态`（顶部 10 行）
- `mbank/architecture.md`：涉及架构决策时
- `mbank/design.md`：涉及需求确认时
- `mbank/tech-stack.md`：涉及技术选型时
- `mbank/context/*.md`：触及具体模块时，按 quickref 当前焦点索引加载

### 深度层（按需）
- `mbank/discovery.md`：理解前期发散决策时
- `mbank/progress.md` 全文：查历史记录和错误时
- `mbank/archive/`：查历史版本时

**核心原则**：quickref 的 `## 当前焦点` 是 /clear 后的恢复入口，每完成 1 个 Step 必须更新。

## 文件结构

```text
项目根目录/
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
