# 📓 AI Co-Pilot 极客共创日记 | 标准模板 (Template)

> **极客箴言**：*“不迷信 Prompt 魔术，只坚信物理机制、架构品味与就地单测。”*  
> **日记宗旨**：记录我与 AI Agents 在真实的 PAF 商业级项目（或其他项目）中，从混沌设想到完美落地的每一秒挣扎、博弈、重构与成长。

---

## 📅 一、 战役基本信息 (Campaign Meta)

*   **战役日期**：`YYYY-MM-DD`
*   **开发分支 / 战区**：`feat/xxxx` 或 `fix/xxxx`
*   **开发用时**：`~X Hours` (由 AI Coach 或 Tracker 录入)
*   **参战 Agents**：`Claude Code (CLI)` / `Cursor (GUI)` / `Vida` 等

---

## 🛠️ 二、 本日研制单元 (Target Module & Unit)

*   **业务单元**：(例如：估值质控、多层路由、文件挂载、同步监听器)
*   **核心痛点**：(用一句话描述今天必须物理消灭的代码顽疾或要实现的全新特性)

---

## 🧬 三、 架构品味与设计决策 (Architecture & Design Decoupling)

全栈 AI 工程师绝不堆砌垃圾代码。记录你们今天做出的最引以为傲的架构抉择（遵循 **公理 3：约定大于配置减熵定律**）：

*   **Before (原始混沌设想)**：(例如：试图把 OCR 代码直接塞进 core 核心层，产生强耦合)
*   **After (优雅解耦设计)**：(例如：采用 Cosmos 风格的非侵入式事件监听器，保持业务代码 100% 的内聚纯粹)
*   **第一性原理支撑**：(描述你们是如何解耦 I/O、隔离概率性、死守确定性防线的)

---

## 🎯 四、 黄金输入指令 (Golden Prompts & Commands)

记录今天最具有“大模型注意力分配机制物理直觉”的指令（遵循 **公理 1：注意力预算定律**），严防注意力逃逸：

```markdown
[Scope]
- 核心路径: apps/api/src/modules/xxxx
- 忽略路径: dist, node_modules, .git
[Task]
- 描述具体重构逻辑...
[Verification]
- 在重构最末尾强制运行: pnpm test
```

---

## 🤖 五、 AI 教练毒舌评价与改进 (AI Co-Pilot Coaching & Regression Audit)

这不仅是工作记录，更是我的**个人成长红线审计板**。在这里，AI 助理将作为教练，对我的操作进行最无情、最真实的拷问：

### 🚨 痛点剖析 (Antipattern Diagnostic)
*   **AI Coach 毒舌诊断**：(例如：*“你今天贪图省事，又犯了‘注意力发散型’退行性错误，不写 Scope 让大模型盲目遍历。这直接浪费了 70% 的 Token 注意力，导致生成代码产生概率性漂移！”*)

### 🟢 Before ➔ After 纠偏对照
*   **Before (我的发散指令)**：`“帮我理解这个项目，把里面的 bug 改了。”`
*   **After (规范对齐指令)**：`“限制在 valuation-qc 目录下，分析 VFS Phase 1 当前阻断，仅在 guardrail.service 中增加 relativePath 的 startsWith 校验。”`

---

## 🏁 六、 落地战果与就地单测 (Realized Goals & Unit Tests)

我们不接受口头承诺。只有通过物理代码拦截与单元测试，战役才算 `DONE`（遵循 **公理 2：概率与确定性隔离定律**）：

*   **落地物理防线**：(描述具体落盘的手写代码或校验机制)
*   **单测验证指令**：`pnpm test`
*   **测试输出结果**：
    ```text
    ✔ apps/api/src/modules/valuation-qc/__tests__/vfs-playground.spec.ts (1 test passed)
    ✔ apps/api/src/modules/valuation-qc/__tests__/intent-router.spec.ts (3 tests passed)
    ```
*   **最终落盘 PR / Commit 战果**：(例如：PR #242 已合并，本地单测 100% 通过)
