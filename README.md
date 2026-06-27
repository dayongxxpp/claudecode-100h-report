# 🧠 AI-Native Full-Stack Engineer Playbook & Lab

> **项目定位**：全栈 AI 工程师的成长沙盒、实战日志与技术兵器库。  
> **核心使命**：以真实的商业级项目（如 PAF 基金质控系统）为演练场，通过“高频操作落盘 ➔ 失败模式深度复盘 ➔ AI 工具极限评测 ➔ 金融安全最佳实践”，实现从“Prompt 调包侠”向“全栈 AI 架构师”的硬核跨越。

---

## 📂 仓库多维结构设计 (Repository Architecture)

为了完美支撑你**“通过复盘与演练，跃升为全栈 AI 工程师”**的目标，本仓库已被升级重构为结构化的**开发者实验室 (Playbook & Lab)**：

```text
claudecode-100h-report/
├── README.md                           # 🌟 实验室控制台：成长路线图、知识索引与每日看板
├── claudecode-100-hours-report.md      # 🏆 核心里程碑：100小时实战总结与方法论白皮书
├── logs/                               # 📅 每日复盘日志：按天归档的原子级操作、报错与纠偏日记
│   └── 2026-06-27-retro.md             # └── 示例：今天的实战复盘与防坑自检
├── labs/                               # 🧪 AI-Native 实验沙盒：用于演练、测试和开发的原子级代码
│   ├── mcp-tools/                      #     ├── 自制 Model Context Protocol (MCP) 扩展测试
│   ├── vfs-playground/                 #     ├── 虚拟文件系统 (VFS) 边界与路径隔离测试
│   └── prompt-templates/               #     └── 高内聚 System Prompt 预算裁剪与上下文压缩演练
└── templates/                          # 📋 提效工具箱：开箱即用的 .clauderules, .cursorrules 模板
```

---

## 🎯 全栈 AI 工程师四大修炼维度

### 1. 📅 [Daily Logs (每日实战复盘)](./logs/)
*   **记录真相**：由后台 Tracker (ID 10) 于每天 23:00 自动抓取你的历史会话（Dialogue Sessions），自动生成并 Commit 推送。
*   **重点沉淀**：不记录流水账，只归档**高频/亮点 Prompt**、**AI 失败模式 (Failure Modes)** 以及**就地单测通过率**。

### 2. 🧪 [Labs & Sandboxes (演练沙盒)](./labs/)
*   **动手演练**：全栈 AI 工程师的核心竞争力是**为 AI 编写工具**（如 MCP、VFS、Agent Triggers）。
*   **代码演练场**：在这个目录下，你可以编写并存放你用来调教 AI 助理的自动化脚本（如 `sync-spark-cards.py` 演练版、Linter 静态验证器）。

### 3. 🏆 [100-Hour Report (里程碑白皮书)](./claudecode-100-hours-report.md)
*   **方法论沉淀**：当你累积满 100 小时实战后，这里将汇聚成一份极具含金量的行业白皮书，涵盖：
    *   *指令工程（Prompt Engineering）在复杂 Monorepo 中的极限效能上限*；
    *   *AI Agent 失败的 10 大必然模式与防御性编程（Defensive Programming）*；
    *   *金融与高隐私行业的数据内聚终极架构方案*。

---

## 📊 个人成长看板 (Developer Dashboard)

*   **当前段位**：`AI-Native Developer (L3)` ➔ 目标：`Full-Stack AI Architect (L5)`
*   **累计实战时长**：`~12 Hours` (Progress: ▓░░░░░░░ 12%)
*   **今日战役**：[2026-06-27 VFS 边界防御与只读 Session 隔离](./logs/2026-06-27-retro.md)
*   **核心装备库**：
    *   🛡️ **VFS 沙盒路径过滤**：在 `PolicyGuardrailService` 中实现路径防逃逸兜底。
    *   🔒 **SQLITE_OPEN_READONLY**：非侵入式读取本地 `memory.db` 会话历史。
