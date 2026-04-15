# 个人开发偏好

> 此文件由用户手动维护，/mbank 启动时自动读取。

## APP 默认技术栈偏好
- 前端: 根据平台而定（桌面优先，无 Web 框架）
- 后端: 无（本地优先）
- 样式: 不适用
- 数据库: 本地存储 / JSON / SQLite

## 游戏默认偏好
- 引擎: Unity
- 语言: C#
- 异步: UniTask
- 架构模式: Command Pattern + Observer (EventBus) + Service Locator
- 数据: CSV（策划表） → JSON（运行时） + ScriptableObject（静态配置）
- 测试: Unity Test Runner（Edit Mode + Play Mode）

## 工具默认偏好
- 语言: Python / TypeScript
- 交互形式: CLI

## 通用规则
- 优先使用中文注释
- Git commit message 使用英文
- 单文件不超过 200 行
- 禁止 GameObject.Find() / FindObjectOfType()（Unity）
- Unity 序列化字段使用 [SerializeField] private + [Tooltip]
- 不使用 ORM，直接写 SQL 或使用本地文件
- 开发环境: Windows + WSL（WSL 用于 shell 脚本和 Docker）
- 避免过度设计，优先 YAGNI
