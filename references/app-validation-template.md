# 应用验证模板参考

## 用途

为 `/smoke` 和 `/check` 命令提供桌面应用验证的参考模式。根据项目类型选择适用的验证策略。

## 三级验证体系

| 等级 | 命令 | 触发时机 | 内容 |
|------|------|---------|------|
| L0 编译 | 构建命令 | 每次代码变更后 | 编译通过，无构建错误 |
| L1 冒烟 | `/smoke` | 完成 Phase 后或用户显式触发 | 应用启动 + 核心路径可用 |
| L2 完整 | `/check` | Milestone 完成或用户显式触发 | 全部验证命令 + 文档一致性 |

---

## Unity 项目

### L0 编译
```bash
# 命令行构建（无界面模式）
Unity.exe -batchmode -quit -projectPath "[项目路径]" -buildTarget [StandaloneWindows64] -logFile build.log
# 成功标准：exit 0，build.log 无 Error
```

### L1 冒烟（/smoke 使用）
| 检查项 | 方式 | 成功标准 |
|--------|------|---------|
| Edit Mode 测试 | Unity Test Runner 命令行 | 全部通过 |
| Play Mode 启动 | Unity Editor Play Mode | 无 Exception，场景正常加载 |
| 核心交互 | 手动验证（需用户操作） | 按 smoke-test.md 用例清单 |

### L2 完整（/check 使用）
| 检查项 | 命令 |
|--------|------|
| Edit Mode 测试 | `Unity -runTests -testPlatform EditMode -testResults results.xml` |
| Play Mode 测试 | `Unity -runTests -testPlatform PlayMode -testResults results.xml` |

### smoke-test.md 示例（Unity）
```markdown
## 启动
**命令**: 在 Unity Editor 中进入 Play Mode
**成功标志**: 主场景加载，无红色 Exception 输出
**超时**: 30s

## 核心冒烟用例
1. [ ] 进入 Play Mode 无报错 — 手动
2. [ ] 主菜单可正常显示 — 手动
3. [ ] 核心游戏循环可启动 — 手动
```

---

## Web / Electron 项目

### L0 编译
```bash
npm run build
# 成功标准：exit 0
```

### L1 冒烟（/smoke 使用）

**有 Playwright MCP 时（推荐）**：
1. 启动应用：`npm start` / `npm run electron`
2. 等待首屏加载（检查特定元素出现）
3. 执行核心交互（导航、基本功能验证）
4. 截图保存为证据
5. 关闭应用，验证无残留进程

**无 Playwright MCP 时（手动验证）**：
列出 smoke-test.md 中的用例清单，供用户逐一确认。

### L2 完整（/check 使用）
```bash
npm test          # 单元测试
npm run e2e       # E2E 测试（如有）
npm run lint      # 代码检查
```

---

## 通用桌面应用（Windows）

### 进程启动检查
```powershell
# PowerShell - 检查应用是否在启动后存活
$proc = Start-Process -FilePath "[app.exe]" -PassThru
Start-Sleep -Seconds 5
if ($proc.HasExited) {
    Write-Error "App crashed on startup (exit code: $($proc.ExitCode))"
    exit 1
} else {
    Write-Output "App running (PID: $($proc.Id))"
    $proc.Kill()
}
```

### WSL / Linux 应用
```bash
# 后台启动并检查进程存活
./app &
APP_PID=$!
sleep 3
if kill -0 $APP_PID 2>/dev/null; then
    echo "App running (PID: $APP_PID)"
    kill $APP_PID
else
    echo "App crashed on startup"
    exit 1
fi
```

---

## /smoke 命令执行流程参考

```
1. 读取 mbank/smoke-test.md
   ├── 找到"启动命令"和"成功标志"
   └── 找到"核心冒烟用例"列表

2. 执行启动命令
   ├── 成功：继续验证
   └── 失败（崩溃/超时）：报告 FAIL + 输出错误日志

3. 验证核心冒烟用例
   ├── 有自动化工具：执行并收集结果
   └── 无自动化工具：列出检查点，要求用户确认

4. 输出结果
   ├── PASS：全部通过
   ├── PARTIAL：启动正常但部分用例失败
   └── FAIL：启动失败或阻断性问题
```

**Iron Law**：必须有运行结果的新鲜证据，不得仅基于代码阅读推测应用状态。
