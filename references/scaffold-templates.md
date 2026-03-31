# `/scaffold` 模板

## `AGENTS.md` 最小模板

```markdown
# 项目工作规则

## 冷启动必读（仅此一个文件）
1. `mbank/quickref.md` — 含当前状态、关键路径、踩坑速览

## 按需阅读（触及相关主题时读取）
- 架构决策、组件职责 → `mbank/architecture.md`
- 产品需求、MVP 范围 → `mbank/design.md`
- 技术栈约束 → `mbank/tech-stack.md`
- 完整进度历史 → `mbank/progress.md`
- 具体模块实现 → `mbank/context/{模块}.md`
- 已归档里程碑 → `mbank/archive/v{版本}.md`

## 通用约束
- 单文件不超过 200 行，超出时优先拆分
- 严格遵守 `mbank/tech-stack.md`
- 未经确认不要引入额外第三方包

## 文档维护要求
- 每完成 1 个 step，同步更新 `mbank/quickref.md` 的当前状态
- 完成重大功能后更新 `mbank/architecture.md`
- 触发条件满足时补充 `mbank/context/*.md`（见 SKILL.md 开发规则）
```

## `CLAUDE.md` 兼容模板

```markdown
# Compatibility Notice

本项目的 agent 工作规范以 `AGENTS.md` 为准。
在生成、修改或重构代码前，先阅读 `AGENTS.md`。
```

## `mbank/implementation-plan.md` 模板

```markdown
# [项目名称] - 实施计划

> **版本**: v1.0
> **复杂度评估**: [轻量/标准/复杂]
> **TDD**: [是/否]

## Phase 0: 项目初始化

### Step 0.1 - [任务名称]
**目标**: [一句话描述]
**测试**: [先写什么测试，测试文件路径]
**操作**: [具体指令，不含代码]
**验证命令**: [如 `npm test`、`pytest`、`go test ./...`]
**验证标准**: [预期结果]

## Milestone 1: [名称]
- [ ] 任务 A
- [ ] 任务 B
```

## `mbank/quickref.md` 模板

```markdown
# [项目名称] - 快速参考

## 当前状态
<!-- 每完成 1 个 step 同步更新，限 5 行 -->
**阶段**: Phase 0
**当前 Step**: 等待开始
**最后完成**: 无
**阻塞点**: 无
**最后更新**: [日期]

## 关键路径
| 功能 | 入口 | 核心文件 | 深度上下文 |
|------|------|----------|-----------|
| (开发时填充) | | | → `mbank/context/xxx.md` |

## 踩坑速览
| 坑 | 模块 | 详情 |
|----|------|------|
| (遇到时填充) | | → `mbank/context/xxx.md` |

## 深入阅读指引
| 当你需要了解... | 读这个文件 |
|----------------|-----------|
| 架构决策、组件职责 | `mbank/architecture.md` |
| 产品需求、MVP 范围 | `mbank/design.md` |
| 技术栈约束 | `mbank/tech-stack.md` |
| 完整进度历史 | `mbank/progress.md` |
| 某模块实现细节 | `mbank/context/{模块}.md` |
| 已归档的里程碑 | `mbank/archive/v{版本}.md` |
```

## `mbank/progress.md` 模板

```markdown
# [项目名称] - 开发进度

## 当前状态
<!-- 保持 10 行以内 -->
**阶段**: Phase 0
**当前任务**: 等待开始
**最后更新**: [日期]

## 已完成任务
(暂无)

## Errors Encountered
| 错误 | 尝试次数 | 解决方案 |
|------|----------|----------|
```
