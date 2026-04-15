# mbank 工作流

一个用于结构化项目开发的 Claude Code Skill。通过文档驱动、分层上下文管理和子代理委托，帮助 AI 理解项目上下文、遵循开发规范、追踪进度，并在大型代码库中保持高效。

## 安装

```bash
git clone https://github.com/Aitcmb/mbank.git ~/.claude/skills/mbank
```

---

## 命令速览

| 命令 | 阶段 | 作用 |
|------|------|------|
| `/discover` | 前期 | 从模糊想法出发，澄清痛点、重构问题、生成 design.md |
| `/mbank` | 规划 | 读取 design.md，做正式澄清和复杂度评估，生成 tech-stack.md |
| `/scaffold` | 初始化 | 生成项目骨架文档（AGENTS.md、implementation-plan 等） |
| `/check` | 验证 | 执行实施计划中的验证命令 + 文档一致性检查 |
| `/smoke` | 验证 | 启动桌面应用，检查阻断性问题，报告 PASS/FAIL/PARTIAL |
| `/archive` | 归档 | 里程碑封版，归档已完成任务，重置进度 |

**推荐顺序**：
- 想法模糊：`/discover` → `/mbank` → `/scaffold` → 开发 → `/smoke` / `/check` → `/archive`
- 已有 design.md：`/mbank` → `/scaffold` → 开发
- 已完成骨架：直接按实施计划开发

---

## 快速开始

### Step 1：配置个人偏好（一次性）

编辑 `~/.claude/skills/mbank/defaults.md`，填写你的技术栈偏好。配置后所有新项目都会自动参考：

```markdown
## 游戏默认偏好
- 引擎: Unity
- 语言: C#

## 工具默认偏好
- 语言: Python / TypeScript

## 通用规则
- 优先中文注释
- 单文件不超过 200 行
```

### Step 2：在项目目录中启动

如果想法还比较模糊：

```
/discover
```

Claude 会引导你完成痛点澄清、问题重构、范围分叉（最小切口版 / 平衡版 / 愿景版），生成 `mbank/discovery.md` 和 `mbank/design.md`。

如果你已经清楚要做什么，直接创建 `mbank/design.md` 后执行：

```
/mbank
```

### Step 3：生成项目骨架

```
/scaffold
```

根据复杂度生成对应文件集：

| 模式 | 条件 | 生成文件 |
|------|------|---------|
| 轻量 | <3 个文件/模块，无后端 | AGENTS.md + design.md + progress.md |
| 标准 | 多模块，需要架构设计 | 全套 + mbank/context/ |
| 复杂 | 大型游戏/多系统交互 | 全套 + 模块 spec + 子系统 context |

### Step 4：开发

告诉 Claude 按实施计划开发。Claude 会：
- 先读 `quickref.md` 的 `## 当前焦点` 恢复上下文
- 按需加载相关模块的 context 文件
- 每完成 1 个 Step 自动更新 quickref 和 progress
- 当同一 Phase 有 2+ 个独立 Step 时，自动切换为并行子代理执行

### Step 5：验证

```
/smoke   # 应用级冒烟测试（启动 → 核心路径 → PASS/FAIL）
/check   # 完整验证（运行所有验证命令 + 文档一致性）
```

### Step 6：归档

```
/archive
```

---

## 命令详解

### `/discover`

用于从"想做什么"收敛成可执行的 `mbank/design.md`。

**触发时机**：想法还不清晰，或已有 design.md 但仍只是功能清单。

**核心流程**：
1. **痛点澄清**：明确谁遇到问题、在哪个场景、当前怎么做、为什么不够
2. **问题重构**：用一句话改写成真正要解决的问题
3. **隐含能力提取**：区分核心能力、支撑能力、暂不纳入范围
4. **范围分叉**：必须给出 3 条路线（最小切口版 / 平衡版 / 愿景版），各写清楚解决什么、风险是什么
5. **收敛**：生成 `mbank/discovery.md` 和 `mbank/design.md`

**结束条件**：用户、场景、痛点明确，产品前提已确认，已比较 3 条路线，design.md 草稿已生成。

---

### `/mbank`

把 `mbank/design.md` 转成可执行的技术方案。

**核心流程**：
1. 读取 `defaults.md` 个人偏好
2. 判断项目类型（APP / 游戏 / 工具），用对应清单补齐缺失信息
3. 评估复杂度（轻量 / 标准 / 复杂）
4. 生成 `mbank/tech-stack.md`，展示给用户确认

**澄清清单（按类型）**：

APP 类：目标平台、离线需求、认证方式、数据存储、实时功能需求

游戏类：核心循环、平台和输入方式、2D/3D 与美术方向、联网需求、引擎偏好、音频需求

工具类：交互形式、输入输出格式、是否跨平台、目标用户

---

### `/scaffold`

生成项目骨架文档。需先完成 `/mbank` 确认技术栈。

**生成文件（标准+模式）**：

```
项目根目录/
├── AGENTS.md                    # 项目规则 + 会话恢复协议（每次对话必读）
├── CLAUDE.md                    # 兼容入口，指向 AGENTS.md
├── .claude/
│   └── rules/                   # 全局硬规则（跨模块不变约束）
└── mbank/
    ├── design.md                # 设计文档
    ├── tech-stack.md            # 技术栈约束
    ├── quickref.md              # 上下文路由层（含当前焦点）
    ├── architecture.md          # 完整架构文档
    ├── implementation-plan.md   # 实施计划（Phase/Step 结构）
    ├── progress.md              # 进度追踪
    ├── smoke-test.md            # 应用冒烟测试配置（APP/游戏类）
    ├── context/                 # 模块深度上下文（开发阶段按需生成）
    └── archive/                 # 里程碑归档
```

**AGENTS.md 包含**（~55 行）：
- 会话恢复协议（/clear 后按什么顺序读什么文件）
- Compact 保留指令（compact 时必须保留的信息）
- TDD 纪律
- 子代理委托规则
- 验证规则（Iron Law）
- 文档同步纪律

---

### `/check`

执行完整验证（仅用户显式触发）：
1. 读取 `mbank/implementation-plan.md`
2. 找到所有已完成步骤的验证命令
3. 依次执行，汇报通过/失败
4. 对失败项提出修复建议
5. 执行完整文档一致性检查

---

### `/smoke`

轻量级应用冒烟测试（仅用户显式触发）：
1. 读取 `mbank/smoke-test.md`
2. 执行启动命令，检查进程存活
3. 如有 Playwright MCP 等自动化工具，执行冒烟用例
4. 如无，列出需用户手动验证的检查点
5. 输出结果：**PASS** / **FAIL** / **PARTIAL**

> **Iron Law**：必须有运行结果的新鲜证据，不得仅基于代码阅读推测应用状态。

**三级验证体系**：

| 等级 | 触发 | 内容 |
|------|------|------|
| L0 编译 | 每次变更 | 构建通过，无错误 |
| L1 冒烟 | `/smoke` | 应用启动 + 核心路径 |
| L2 完整 | `/check` | 全部验证命令 + 文档一致性 |

---

### `/archive`

里程碑封版（不替代 git）：
1. 将 progress.md 的已完成任务移入 `mbank/archive/v{版本号}.md`
2. 重置 progress.md 的当前状态
3. 更新 architecture.md 版本号
4. 检查 quickref.md 完整性
5. 生成阶段总结

适用于：MVP 完成、一个 milestone 完成、准备进入下一大阶段。

---

## 上下文架构

### 分层加载（/clear 后只需读 2 个文件）

| 层 | 文件 | 加载时机 |
|----|------|---------|
| **必读层** | `AGENTS.md` | 每次对话 |
| **必读层** | `mbank/quickref.md` | 每次对话（上下文路由层） |
| 按需层 | `mbank/progress.md` 当前状态 | quickref 指引 |
| 按需层 | `mbank/architecture.md` | 涉及架构决策时 |
| 按需层 | `mbank/design.md` | 涉及需求确认时 |
| 按需层 | `mbank/tech-stack.md` | 涉及技术选型时 |
| 按需层 | `mbank/context/*.md` | 触及对应模块时 |
| 深度层 | `mbank/progress.md` 全文 | 查历史记录时 |
| 深度层 | `mbank/archive/` | 查历史版本时 |

**关键设计**：`quickref.md` 是上下文路由层，不是内容文档。它的 `## 当前焦点` 记录当前 Phase/Step 和需要加载的 context 文件，是 /clear 后的恢复入口。

### quickref.md 结构

```markdown
## 当前焦点
<!-- 每完成 1 个 Step 立即更新 -->
**阶段**: Phase 1: 核心功能
**当前任务**: Step 1.2 - 数据层实现
**需要读取**: mbank/context/database.md

## 关键路径
| 功能 | 入口 | 核心文件 | 上下文 |
...

## 踩坑速览
| 坑 | 模块 | 详情 |
...
```

### 文档同步规则

- **每完成 1 个 Step**：更新 progress.md 当前状态 + quickref.md 当前焦点
- **每完成 3 个 Step**：同步 quickref 关键路径表、architecture 组件表、检查 context/ 覆盖率

---

## 子代理并行执行

当同一 Phase 有 **2+ 个互不依赖的 Step** 时，自动切换为并行子代理模式：

```
主上下文（调度器）
├── 子代理 A → Step 1.2（UI 模块）
├── 子代理 B → Step 1.3（数据层）
└── 子代理 C → Step 1.4（网络层）
       ↓（各自完成后报告）
主上下文：独立验证 → 更新文档
```

**委托规则**：
- 子代理 prompt 由主上下文直接注入（Step 全文 + 相关 context + 技术栈约束），不让子代理自己读 mbank/ 文档
- 子代理报告 4 种状态：`DONE` / `DONE_WITH_CONCERNS` / `NEEDS_CONTEXT` / `BLOCKED`
- 同一文件只能被一个子代理修改（有冲突则改为顺序执行）
- 轻量模式（<3 文件）不使用子代理

---

## 文档写入规则

| 写入位置 | 放什么 | 不放什么 |
|---------|--------|---------|
| `.claude/rules/` | 跨模块不变约束（禁令/风格） | 模块实现细节 |
| `mbank/context/` | 模块职责、流程、踩坑详情 | 全局规则 |
| `mbank/quickref.md` | 当前焦点、关键路径索引、踩坑摘要 | 详细踩坑内容 |
| `mbank/architecture.md` | 完整组件职责表、数据流、设计决策 | 重复 quickref 的摘要 |

---

## 参考文件

| 文件 | 用途 |
|------|------|
| `defaults.md` | 个人技术栈偏好（/mbank 读取） |
| `references/scaffold-templates.md` | /scaffold 生成文件的模板 |
| `references/app-validation-template.md` | /smoke 的验证参考（Unity/Web/通用桌面） |
| `references/design-template.md` | /discover 生成 design.md 的模板 |
| `references/discovery-template.md` | /discover 生成 discovery.md 的模板 |

---

## 许可证

MIT
