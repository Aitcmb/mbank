---
name: mbank
description: "mbank 结构化开发工作流：/discover → /mbank → /scaffold → /check → /archive"
---

# mbank 工作流

按需读取引用文件：
- `/discover` → `references/discover-flow.md`、`references/discovery-template.md`、`references/design-template.md`
- `/mbank` → `references/mbank-flow.md`
- `/scaffold` → `references/scaffold-templates.md`

## 命令

| 命令 | 作用 |
|------|------|
| `/discover` | 从想法或粗糙需求出发，完成痛点澄清、问题重构、范围分叉，生成 `mbank/discovery.md` 与 `mbank/design.md` |
| `/mbank` | 读取 `mbank/design.md`，做正式澄清、复杂度评估，生成 `mbank/tech-stack.md` |
| `/scaffold` | 根据复杂度和技术栈生成项目骨架文档 |
| `/check` | 用户显式触发：文档一致性验证 + 可选回归测试 |
| `/archive` | 用户显式触发：里程碑封版、归档、压缩活跃文档 |

## 推荐顺序

- 想法还模糊：`/discover` → `/mbank` → `/scaffold`
- 已有成熟 design.md：`/mbank` → `/scaffold`
- 已完成骨架初始化：直接按实施计划进入开发

---

## /discover

把"想做什么"收敛成可执行的 `mbank/design.md`。

- **触发**：想法模糊，或已有 design.md 但仍只是功能清单
- **流程**：读取已有材料 → 痛点澄清 → 问题重构 → 隐含能力提取 → 范围分叉（3 条路线）→ 收敛
- **详细步骤**：见 `references/discover-flow.md`
- **输出物**：`mbank/discovery.md`、`mbank/design.md`
- **结束条件**：用户/场景/痛点明确、产品前提已确认、已选定推荐路线、design.md 草稿已生成

---

## /mbank

把 `mbank/design.md` 转成可执行的技术方案。

- **输入守卫**：无 design.md 或内容不完整 → 先执行 `/discover`
- **流程**：读取个人偏好 → 正式澄清（按项目类型）→ 复杂度评估 → 生成技术栈
- **详细步骤与澄清清单**：见 `references/mbank-flow.md`
- **输出物**：`mbank/tech-stack.md`（需用户确认）
- **结束条件**：项目类型已判定、缺失信息已补齐、复杂度已评估、tech-stack.md 已确认

---

## /scaffold

技术栈确认后生成项目骨架文档。模板见 `references/scaffold-templates.md`。

- **生成内容**：`AGENTS.md`、可选 `CLAUDE.md`、`implementation-plan`、`progress`、`architecture`、`quickref`、`context/`

### 开发阶段规则

1. 冷启动只读 `quickref.md`（含当前状态 + 路径索引），按需展开其他文件
2. 标准/复杂模式严格 TDD：Red → Green → Refactor
3. 验证通过前不进入下一步
4. 每完成 1 个 step，同步更新 `quickref.md` 的当前状态区块
5. 完成重大功能后更新 `architecture.md`
6. 每完成 3 个 step，自动执行一次轻量文档一致性检查
7. **context/ 写入机制**：满足以下任一条件时写入 `mbank/context/{模块名}.md`：
   - 模块代码超过 3 个文件
   - 遇到踩坑并解决（需记录原因 + 正确做法）
   - 模块间有非显而易见的依赖或约束
   - 格式：职责(一句话) / 关键文件 / 核心流程 / 约束与踩坑
   - 写入后同步更新 `quickref.md` 的踩坑速览和关键路径表
8. **阶段结束压缩**：每个 Phase 完成后，将推导过程从活跃文档删除，仅保留结论和决策记录

---

## /check

用户显式触发。核心职责是**文档一致性验证**（代码回归测试为可选，因 TDD 已在开发中覆盖）。

1. 验证 `quickref.md` 的关键路径：入口文件是否存在、核心文件路径是否正确
2. 验证 `architecture.md` 与代码是否对齐：是否有未记录的新组件或已删除的旧组件
3. 验证 `context/` 覆盖率：所有已完成模块是否都有对应的 context 文档
4. 验证 `progress.md` 是否漏记已完成任务
5. 一致性检查方式：**编写并运行临时验证脚本**，而非仅问"是否一致"
6. 可选：运行所有已完成步骤的验证命令做回归测试
7. 汇报通过/失败结果，对失败项提出修复建议

---

## /archive

用户显式触发的里程碑封版。

1. 确认里程碑名称和版本号
2. 将 `progress.md` 已完成任务移入 `mbank/archive/v{版本号}.md`
3. 重置 `progress.md` 当前状态
4. 更新 `architecture.md` 版本号
5. 生成阶段总结：已完成功能、已知问题、下阶段方向
6. **压缩活跃文档**：删除 design.md 中已实现功能的详细描述，仅保留功能名称和状态标记

---

## 反压与上下文管理

### 工具输出管理
- 所有验证/构建命令输出超过 20 行时，必须 `| head -20` 或 `grep -C 5 'error\|fail'`
- 全局搜索（grep 超过 3 个文件）、大规模重构：使用子 Agent，主会话仅接收压缩后的结论
- 禁止将完整日志粘贴到对话中

### 压缩保留优先级
当需要压缩上下文时，按以下优先级保留（高到低）：
1. AGENTS.md 规则、当前任务目标
2. quickref.md 当前状态与关键路径
3. 当前 step 的测试结果和错误信息
4. design.md 的 MVP 范围和关键约束
5. 历史 step 的完成状态（仅保留通过/失败标记）

丢弃：已完成 step 的详细推导、旧的错误调试过程、discovery.md 的发散内容

### 上下文预算警报
- 对话超过 15 轮或估计上下文占用超过 50% 时，主动提示用户：
  "建议执行 /archive 封版当前进度，然后开启新对话继续。"
- 单个 Phase 内对话超过 25 轮时，强制建议压缩
