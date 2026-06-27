# 🏆 Claude Code 100 小时实战方法论白皮书

> **简介**：汇总了使用 Claude Code 100 小时内的核心方法论、AI-Native 研发范式、失败防御性编程以及多智能体在资产管理等金融敏感场景下的工程落地规范。

---

## 🚀 核心方法论 (Methodologies)

### 1. 指令工程：三段式高内聚 Prompt 标准
当进入复杂 Monorepo 库开发时，传统的模糊自然语言会引发 AI 迷航。我们推行 **`[Scope] ➔ [Task] ➔ [Verification]`** 的三段式高内聚指令标准：
*   **[Scope]**：限制 AI 扫描和读写的目录，拉起物理白名单。
*   **[Task]**：规定极简的、有明确 Definition of Done 的任务链路。
*   **[Verification]**：强制要求在重构完后，就地拉起 `pnpm test` / `vitest` / `jest` 运行单元测试，将错误拦截在本地阶段。

### 2. 失败防御性编程 (Defensive Agentic Programming)
AI 代理在读写文件时，存在天生的“幻觉边界”。为防止其写出越界、污染或破坏系统的代码，我们要求在 Handler 层面设置**物理第二防线（Physical Double-Check）**：
*   在处理路径落盘时，Handler 必须对相对路径进行严格清洗（去 `..`，过滤绝对路径首斜杠）。
*   强制使用物理断言，对合成路径执行 `startsWith(sandboxDir + path.sep)`。一旦逃逸，立即主动抛错阻断写盘，而不能依赖 AI 自我纠偏。

### 3. 金融级 Strict Session-Only Scoping 规范
在面临严格安全审计的对账、质控行业，任何对桌面屏幕、窗口的高频截屏或录制都具有极高的数据安全风险。
*   **沙盒会话限定**：我们彻底切断一切前台屏幕和录像监控。
*   **时空记忆穿梭**：仅允许 AI 通过 `OPEN_READONLY` 方式，只读读取本地隐藏的 `.db` 数据库中的 `memory_sessions` 对话表。
*   通过读取历史会话的主题、结论与摘要，完成上下文的精准记忆还原。

---

## 📈 历程索引与实践归档

*   📅 **[2026-06-27 | Agent-VFS 路径逃逸洗牌重构与类名冲突化解](./logs/2026-06-27-retro.md)**：首次在 `feat/agent-vfs-phase1` 开发分支中践行“两阶段 VFS 异步分离”和“物理逃逸清洗防线”。
