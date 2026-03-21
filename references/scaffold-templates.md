# `/scaffold` 模板

## `AGENTS.md` 最小模板

```markdown
# 项目工作规则

## 开始任何编码前必须阅读
1. `mbank/quickref.md`
2. `mbank/architecture.md`
3. `mbank/design.md`
4. `mbank/tech-stack.md`
5. `mbank/progress.md` 的 `## 当前状态`

## 通用约束
- 单文件不超过 200 行，超出时优先拆分
- 严格遵守 `mbank/tech-stack.md`
- 未经确认不要引入额外第三方包

## 文档维护要求
- 完成重大功能后更新 `mbank/architecture.md`
- 完成重大功能后更新 `mbank/quickref.md`
- 需要时补充 `mbank/context/*.md`
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
