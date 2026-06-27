# 🚀 Claude Code / Cursor 100小时实战报告

> **定位**：重度 AI-Native 开发者在资产管理、金融质控、大工程重构场景下的极客实践沉淀。  
> **真相主线**：用真实的生产级代码、报错、纠偏与测试闭环，打破 AI 编程幻觉，沉淀人机共生的高吞吐工程经验。  
> **声明**：本仓库由后台自动化 Tracker (ID 10) 每日夜间无感提取会话并自动更新推送。

---

## 🧭 报告大纲 (Chapters)

*   **[章节 1：高频使用场景 (Scenarios)](#-章节-1高频使用场景)**：代码生成、数据清洗、跨 PR 审查、文档与测试闭环。
*   **[章节 2：失败模式分类 (Failure Modes)](#-章节-2失败模式分类)**：幻觉、工具冲突、上下文丢失、指令漂移的真实边界。
*   **[章节 3：与 Cursor/Copilot/Augment 差异化对比](#-章节-3与-cursorcopilotaugment-差异化对比)**：CLI vs GUI，心智摩擦与 Token 预算的终极博弈。
*   **[章节 4：金融与高隐私场景使用心得](#-章节-4金融与高隐私场景使用心得)**：Prisma 软删除、沙盒隔离、只读 SQLite 与 Strict Session-Only 会话沙盒规范。

---

## 📅 每日实战复盘日志 (Daily Practical Logs)

### 🔴 2026-06-27 | 战役：Agentic VFS 重构、路径溢出防御与跨 PR 合并

#### 1. 🎯 章节 1：高频使用场景

##### 📂 【场景 A】跨 PR (#232 & #233) 深度审查与合并演练
*   **输入 Prompt**：
    > *"Complete Code Review, Fix Issues, and Rerun CI for PRs 232 and 233"*
*   **AI 行为与产出**：
    1.  AI 自动调用 `gh pr view 233` 与 `gh pr view 232`，读取 PR 的合并阻碍（其中 #232 涉及 `pnpm audit` 检测出的高危安全漏洞，#233 涉及 `Agent-VFS Phase 1` 的原子落盘功能）。
    2.  AI 自动识别出 `#233` 中新增的 `RegulationMountHandler` 与 `RegulationSyncListener` 结构。
    3.  自动执行 `pnpm --filter api test`，就地跑通了所有合规提取与挂载落盘的 `spec` 单元测试。

##### 📂 【场景 B】VFS 第二道防线：路径逃逸清洗重构
*   **输入 Prompt**：
    > *"Adding contextPolicy to Expert registry... / 优化 policy-guardrail 物理落盘防逃逸"*
*   **AI 行为与产出**：
    1.  由于 `PolicyGuardrailService` 在保存提取出的 regulations markdown 文件时，原本使用 `path.basename`。
    2.  用户输入指令指出这样会导致多子目录下的同名文件碰撞覆盖。
    3.  AI 自动将其修改为保留相对目录，并配合 `PolicyGuardrailService.sanitizeRegRelPath` 进行 `..`、绝对路径与空段清洗。
    4.  通过 `path.sep` 进行物理边界检查，若文件尝试跳出 `skills/knowledge/regulations` 根目录，立即抛出 `regulations path escaped root` 阻断。

---

#### 2. ❌ 章节 2：失败模式分类 (Failure Modes)

##### 🛠️ 【模式 A】工具调用与沙盒越界冲突 (Tool Call Errors)
*   **真实案例**：
    *   在开发 `feat/agent-vfs-phase1` 分支时，AI 尝试写入绝对路径 `/paf/regulations`，导致底层 `path.resolve` 直接绕过了 `FileSystemSandboxService` 的虚拟沙盒根。
*   **后果**：触发沙盒安全阻断，写盘失败。
*   **纠偏方案 (Correction)**：
    *   在 `RegulationMountHandler` 中强制进行路径前缀洗牌，只向 `FileSystemSandbox` 传递不带首斜杠的相对路径（如 `paf/regulations/amac_2024.md`）。**千万不要让 AI 直接操作系统的绝对路径，必须由 Handler 在最后一步完成沙盒物理映射。**

##### 📋 【模式 B】重名类名覆盖冲突 (Name Collision)
*   **真实案例**：
    *   在尝试设计 Outbound 质控日志同步时，AI 尝试直接在 `valuation-qc/skill/` 目录下生成 `regulation-sync.listener.ts`，未检测到该分支上已存在同名的 VFS 同步监听器。
*   **后果**：若执行，将导致新规自动提取功能被静默覆盖损毁。
*   **纠偏方案 (Correction)**：
    *   进行前置 Git 差异检查。将日志监听器改名为 `OpenChronicleDailySyncListener`，落盘文件重命名为 `openchronicle-daily-sync.listener.ts`，物理隔离开发热区。

---

#### 3. ⚖️ 章节 3：与 Cursor/Copilot/Augment 差异化对比

| 维度 | Claude Code (CLI) | Cursor (GUI) | Augment (Cosmos) |
| :--- | :--- | :--- | :--- |
| **首选场景** | 快速跑单测、单文件极致重构、Git 批量提交、独立 VFS 挂载逻辑编写。 | 多文件大图景速读、UI 视觉热重载、端到端页面对账。 | 生产级 PR 冲突审查、DAG 状态验证。 |
| **Token 消耗** | **极低**（在裁剪了 `CLAUDE.md` 的垃圾上下文后，单次耗时压缩 90%）。 | **中等**（每次修改都可能携带大量前台不相关的 TS 定义上下文）。 | **高**（走完整的工程级审查链）。 |
| **交互摩擦** | 使用 `--dangerously-skip-permissions` 后实现**零摩擦自动化**，适合无人值守的闭环开发。 | 需要频繁在代码编辑框与 Chat 栏之间切换，手动点击 Accept。 | 深度集成于 GitHub 平台。 |

---

#### 4. 🏦 章节 4：金融与高隐私场景使用心得

##### 🔒 【法则】Strict Session-Only 会话沙盒隔离
*   **核心痛点**：在金融质控中，将前台屏幕录像、OCR 文本或敏感文件上传至 AI 云端解析，会直接触碰企业合规红线。
*   **极客解法**：
    *   **切断屏幕活动监控**。将 OpenChronicle 感知引擎的权限**绝对限制在“本助理历史会话记录 (Dialogue Session Logs)”内**。
    *   后端仅通过**只读方式（SQLITE_OPEN_READONLY）**调阅本地隐藏的 `/memory.db` 数据库中的 `memory_sessions` 表。
    *   这样在不监控任何用户屏幕隐私、不截屏、不录屏的前提下，AI 仅凭对往期对话主题、核销摘要的回溯，就能实现跨 Session 的“上下文记忆穿梭”，实现了物理级绝对合规。

---

> 💡 **每日一言**：*"Convention over configuration. 不要花 Token 去给 AI 重新解释 Rails，在干净、符合约定的代码库里，AI 已经知道了所有的秘密。"*
