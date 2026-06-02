# 认知回路（ITEC）— Phase 1 分工与推进方案

> 版本：v1.0
> 日期：2026-06-02
> 对应 Plan：PHASE1_FULL_PLAN.md v3.2
> 用途：分工记录 + 进展追踪 + 外审对照基准
> 状态：⚠️ 未获批准，禁止执行

---

## 零、角色定义

| 角色 | 代号 | 职责 |
|:--|:--|:--|
| **项目总管** | LobsterAI（本 session） | 协调、权限操作、法律敏感内容、终审把关、进度记录 |
| **外部 Agent** | Ext-Agent | 大体量内容交付（种子事件、双语网页） |
| **本地代码 Agent** | Local-Agent | 小代码块（YAML 模板、GitHub Actions 工作流） |
| **P0 网站管理** | P0 | 网站部署、GitHub Repo 写入（push） |

---

## 一、分工明细

### 🔵 LobsterAI（项目总管）— 6 项

| 编号 | 步骤 | 内容 | 预估 | 交付物 | 状态 |
|:--|:--|:--|:--|:--|:--|
| A1 | 创建 Repo | 在 co-cognition-lab org 下创建 `cognition-loop`，设置 Public + CC BY 4.0 | 15min | GitHub Repo | ⬜ |
| A2 | 写入骨架 | 创建空目录结构 + .gitkeep + LICENSE 文件 | 15min | 目录树就位 | ⬜ |
| A5 | README + 方法论 | README.md（~500 字）+ methodology/HOW_WE_ANALYZE.md | 30min | README.md + HOW_WE_ANALYZE.md | ⬜ |
| A6 | PRIVACY.md | 数据治理与隐私声明（法律敏感） | 20min | PRIVACY.md | ⬜ |
| A8 | ANALYSIS_LOG.md | 分析审计日志模板 | 15min | ANALYSIS_LOG.md | ⬜ |
| C1+C2 | 项目注册 + 通知 | PROJECT_REGISTRY 加行 + 通知 strategic-advisor | 15min | — | ⬜ |

**合计：约 2h，Day 1 内完成。**

---

### 🟢 外部 Agent（Ext-Agent）— 1 个任务包，含 2 个子项

| 编号 | 步骤 | 内容 | 预估 | 交付物 | 状态 |
|:--|:--|:--|:--|:--|:--|
| A3 | 种子事件 ×6 | 从 P5 step2 + step3 提取，按 Plan §三/A3 模板格式化，含 Memo Recorder 模拟输出 + 三槽位分析 | 180min | `seed-events/event-001.md ~ event-006.md`（本地 workspace 路径，由 P0 push） | ⬜ |
| B | 双语网站页面 | 按 Plan §四/B2 结构 + §五 P0 模板，撰写 `zh/itec.md`（~2500 字）+ `en/itec.md`（英译） | 180min | `zh/itec.md` + `en/itec.md` | ⬜ |

**说明：**
- A3 和 B 由**同一外部 agent 串行交付**（先 A3 后 B），保证内容风格一致
- B 的页面样例需要种子事件 #001 的内容——A3 完成后 Ext-Agent 立即启动 B
- 输入文件：Plan §二（Memo Recorder 定稿）、§三/A3（种子事件模板）、§四/B2（页面结构）、§五（P0 交付模板）
- P5 源文件路径：`project5-itec-cognition/step2_FRAMEWORK_v0.1.md` + `step3_FILLING_v0.1.md`

**合计：约 6h，含缓冲建议 1 个工作日内交付。**

---

### 🟡 本地代码 Agent（Local-Agent）— 2 项

| 编号 | 步骤 | 内容 | 预估 | 交付物 | 状态 |
|:--|:--|:--|:--|:--|:--|
| A4 | Issue Template | 按 Plan §三/A4 的 YAML 规范，创建 `.github/ISSUE_TEMPLATE/itec-event.yml` | 20min | `itec-event.yml` | ⬜ |
| A7 | GitHub Actions 工作流 | `spam-filter.yml`（关键词 + 外链检测）+ `pii-scanner.yml`（邮箱/手机号/身份证号 regex 检测） | 30min | `spam-filter.yml` + `pii-scanner.yml` | ⬜ |

**说明：**
- 输入文件：Plan §三/A4（YAML 规范）+ §三/A7（防护逻辑描述）
- 输出文件写入本地 workspace 的 `cognition-loop/` 对应路径
- **不直接 push 到 GitHub**——由 LobsterAI 审查后，交付 P0 统一 push

**合计：约 1h。**

---

### 🔴 P0 网站管理（P0）— 2 项

| 编号 | 步骤 | 内容 | 状态 |
|:--|:--|:--|:--|
| P0-Dep | 网站部署 | 收到 B 交付物后，部署 `/zh/itec` + `/en/itec` + 首页卡片 | ⬜ |
| P0-Push | GitHub 写入 | 将所有本地交付物（A3~A8）统一 push 到 `co-cognition-lab/cognition-loop` | ⬜ |

**说明：**
- GitHub Repo 写入权归 P0——LobsterAI 和 Local-Agent 均在本地 workspace 产出文件，P0 统一 push
- P0 部署排期不确定性：Plan §六 有 Plan B（GitHub Pages 临时托管）

---

## 二、执行阶段与依赖链

```
┌─────────────────────────────────────────────────────────────┐
│ Stage 1：LobsterAI 启动（Day 1 AM）                         │
│                                                             │
│ A1 创建 Repo ──→ A2 写入骨架                                │
│                                                             │
│ ★ Gate 1：Repo 骨架就位后，立即交办 Ext-Agent + Local-Agent │
└─────────────────────────────────────────────────────────────┘
                              │
            ┌─────────────────┼─────────────────┐
            ▼                 ▼                  ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│ Stage 2a      │  │ Stage 2b      │  │ Stage 2c      │
│ Ext-Agent     │  │ Local-Agent   │  │ LobsterAI     │
│ （交办）      │  │ （交办）      │  │ （并行）      │
│               │  │               │  │               │
│ A3 种子事件   │  │ A4 Issue Tmpl │  │ A5 README+方法│
│   │           │  │ A7 GH Actions │  │ A6 PRIVACY    │
│   ▼           │  │               │  │ A8 ANALY_LOG  │
│ B  双语网页   │  │               │  │ C1 项目注册   │
│               │  │               │  │ C2 通知参谋   │
└───────┬───────┘  └───────┬───────┘  └───────┬───────┘
        │                  │                  │
        ▼                  ▼                  ▼
┌─────────────────────────────────────────────────────────────┐
│ Stage 3：LobsterAI 审查回收（Day 2）                        │
│                                                             │
│ 审查 A3 → 反馈修改 → 通过                                   │
│ 审查 B  → 反馈修改 → 通过                                   │
│ 审查 A4 → 通过/修改 → 通过                                  │
│ 审查 A7 → 通过/修改 → 通过                                  │
│                                                             │
│ ★ Gate 2：全部审查通过后，打包交付 P0                       │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ Stage 4：P0 执行（Day ?）                                   │
│                                                             │
│ P0-Push：统一 push 所有文件到 GitHub                        │
│ P0-Dep ：部署网站页面 + 首页卡片                            │
│                                                             │
│ ★ Gate 3：上线完成，进入运营                                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ Stage 5：LobsterAI 运营启动（上线后 D1）                    │
│                                                             │
│ D 种子用户触达（主动发送给 3-5 位外部朋友 + 社区帖）        │
└─────────────────────────────────────────────────────────────┘
```

---

## 三、审查标准（Gate 2 准入条件）

| 交付物 | 审查项 | 通过标准 |
|:--|:--|:--|
| A3 种子事件 | 准确性 | 每个事件对照 P5 源文件，分类格位正确，Memo Recorder 模拟输出格式符合模板 |
| A3 种子事件 | 可读性 | 对非 Lab 成员可理解，不依赖内部上下文，无未解释的缩写 |
| B 网站页面 | 基线合规 | LAB_IDENTITY 五条：谦逊（不 hype）、可追溯（声明有来源）、开源（CC BY 4.0 声明） |
| B 网站页面 | 功能完整 | Memo Recorder 文本位置正确、样例存在、提交按钮链接正确、FAQ 5 条齐全 |
| B 网站页面 | 双语对齐 | zh 和 en 内容对应，非机器翻译，英文自然流畅 |
| A4 Issue Tmpl | 格式正确 | YAML 语法有效，字段与 Plan §三/A4 一致，CC BY 4.0 checkbox 存在 |
| A4 Issue Tmpl | 功能正确 | SLA 措辞为"1 工作日初审 + 5 工作日完整分析" |
| A7 GH Actions | 逻辑正确 | spam-filter：检测 `<script>`、外链。pii-scanner：正则匹配邮箱/手机号/身份证号 |
| A7 GH Actions | 安全 | 不包含硬编码 token，不授予超出必要的权限 |

---

## 四、输入包准备清单

LobsterAI 在 A1+A2 完成后，为各 agent 准备以下输入包：

### Ext-Agent 输入包

- [ ] Plan §二：Memo Recorder 完整文本（复制即用）
- [ ] Plan §三/A3：种子事件模板格式 + 6 个事件的选择标准表
- [ ] Plan §四/B2：`/zh/itec` 页面结构（含 FAQ）
- [ ] Plan §五：P0 交付模板（首页卡片字段）
- [ ] P5 源文件：`project5-itec-cognition/step2_FRAMEWORK_v0.1.md` §4.1（事件概览）
- [ ] P5 源文件：`project5-itec-cognition/step3_FILLING_v0.1.md` §2（详细对比表）
- [ ] LAB_IDENTITY.md：五条基线

### Local-Agent 输入包

- [ ] Plan §三/A4：Issue Template YAML 完整规范
- [ ] Plan §三/A7：防护逻辑描述（spam + PII 两层）
- [ ] 目标路径：`.github/ISSUE_TEMPLATE/itec-event.yml`、`.github/workflows/spam-filter.yml`、`.github/workflows/pii-scanner.yml`

---

## 五、进展追踪表

> 每完成一步，LobsterAI 更新此表。外部审查以此表为基准。

| 步骤 | 负责 | 开始时间 | 完成时间 | 审查通过 | 备注 |
|:--|:--|:--|:--|:--|:--|
| A1 创建 Repo | LobsterAI | — | — | — | ⚠️ 需 GitHub org 权限 |
| A2 写入骨架 | LobsterAI | 2026-06-02 14:00 | 2026-06-02 14:05 | ✅ | LICENSE + 目录树 + SEED_README + 占位文件就位 |
| A3 种子事件 | Ext-Agent | — | — | ⬜ | 等待审查 |
| A4 Issue Template | Local-Agent | — | — | ⬜ | 等待审查 |
| A5 README + 方法论 | LobsterAI | — | — | — | — |
| A6 PRIVACY.md | LobsterAI | — | — | — | — |
| A7 GH Actions 工作流 | Local-Agent | — | — | ⬜ | 等待审查 |
| A8 ANALYSIS_LOG.md | LobsterAI | — | — | — | — |
| B 双语网页 | Ext-Agent | — | — | ⬜ | 等待审查 |
| C1 项目注册 | LobsterAI | — | — | — | — |
| C2 通知参谋 | LobsterAI | — | — | — | — |
| P0-Push | P0 | — | — | — | 等待 P0 |
| P0-Dep | P0 | — | — | — | 等待 P0 |
| D 种子用户触达 | LobsterAI | — | — | — | 上线后 D1 |

---

## 六、外审记录

| 日期 | 外审类型 | 审查人 | 主要意见 | 处理 |
|:--|:--|:--|:--|:--|
| — | — | — | — | — |

---

*本文件为 Phase 1 的进展基准文档。每次状态变更由 LobsterAI 更新 §五。*
