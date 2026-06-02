# 认知回路（ITEC）— Phase 1 完整方案

> 日期：2026-06-02
> 版本：v3.2（GLM 反馈修订版）
> 状态：⚠️ 未获批准，禁止执行
> 上版：v3.1（品牌命名）→ 吸收 GLM 5 条反馈：SLA 分级 / PII 扫描 / 术语揭示块 / 同类项目调研 / 分析审计日志 / 网站分析工具

---

## 零、Phase 1 目标与量化判据

### 定性目标

让认知回路（ITEC）从一个内部项目文件变成一个**有发现路径、有提交入口、有种子数据**的公共参与系统。

核心创新：**用户自己的 LLM 本身就是数据采集工具。** 用户不需要手写事件描述——复制一段 Memo Recorder prompt 给 LLM，LLM 输出结构化 memo，用户审核脱敏后提交。

### 量化成功指标（MUST-06）

| 优先级 | 指标 | 目标值 | 时间窗口 | 数据来源 |
|:------|:-----|:------|:--------|:--------|
| P0 | 外部提交数 | ≥1 个 | 上线后 30 天 | GitHub Issues 标签统计 |
| P1 | 页面访问量 | ≥100 UV | 上线后 30 天 | 网站分析工具 |
| P1 | Issues → 初审完成率 | 100% | 提交后 1 工作日 | GitHub Issues 标签迁移（pending-review → pending-analysis） |
| P1 | Issues → 完整分析完成率 | ≥90% | 提交后 5 工作日 | GitHub Issues 状态迁移（pending-analysis → resolved） |
| P2 | 外部引用/讨论 | ≥1 次 | 上线后 60 天 | 搜索引擎/社交媒体 |

### 内部预警指标

- **"待分析积压超过 10 个"** → 触发流程调整（暂停接收新提交？增加分析人手？）（MUST-04）
- **上线后 60 天仍未获外部提交** → 进入 Phase 1.5 回顾（SHOULD-04）

### 同类项目调研（GLM 反馈新增）

对外传播时会被问到的第一个问题——"你们跟 XX 有什么不同？"

| 项目 | 与我们的关系 | 关键差异 |
|:--|:--|:--|
| **AI Incident Database**（incidentdatabase.ai） | 最接近的同类——收集 AI 系统负面事件 | 他们收集的是"AI 造成伤害"的事件（事故报告型），我们收集的是"LLM 跳过验证"的认知模式（现象记录型）。他们关注后果，我们关注机制 |
| **LMSYS Chatbot Arena** | 众包 LLM 评估 | 他们评"好不好用"，我们收集"怎么出错的"。互补——可以引用他们的排行榜来说明 ITEC 在不同模型上的分布差异（Phase 3+） |
| **Anthropic Collective Constitutional AI** | 公众参与 AI 行为规范 | 他们让公众参与规则制定（"AI 应该怎么做"），我们让公众参与现象收集（"AI 实际上做了什么"）。可以互引为"公众参与 AI 对齐的不同路径" |

**我们的差异化一句话：** 不是事故报告，不是性能排行，不是规则制定——是"这个认知模式有个名字，收集每个人的实例来验证它"。

### Phase 1 结束条件（SHOULD-04）

满足任一即进入 Phase 2：

1. P0 指标达成（第一个外部提交）+ 所有待分析 Issue 清零
2. 上线后 60 天仍未获外部提交 → 进入 Phase 1.5（回顾与调整）

---

## 一、整体架构：双轨道 + 一核心产物 + 统一品牌

```
品牌层：对外统一为「认知回路（ITEC）」/「Cognition Loop (ITEC)」
        认知回路 = 用户能理解的行为闭环（我经历→记录→提交→得到分析→再经历）
        ITEC = Lab 的学术概念，用户在使用中自然接触和接受
        ─────────────────────────────────────────────────
                  ║  核心产物：ITEC Memo Recorder        ║
                  ║  一段 markdown prompt                ║
                  ║  用户复制 → 发给 LLM → 产出 memo    ║
                  ╚══════════════════════════════════════╝
                                  │
            ┌─────────────────────┴─────────────────────┐
            ▼                                           ▼
    轨道 A：GitHub Repo（我来做）             轨道 B：网站呈现（我提供内容，P0 部署）
    co-cognition-lab/cognition-loop          co-cognition.org
    Issue Template + 种子事件 + README       首页卡片 + /zh/itec + /en/itec
    + PRIVACY + 防护机制                     + FAQ
```

**品牌定位：** 体验先行，术语后置。用户先被"认知回路"吸引 → 在参与过程中自然接触 ITEC 概念 → 久而久之 ITEC 变成行业通用术语。

**三者的关系：**
- Memo Recorder 是触发 LLM 记录的工具——不含任何提交 URL，不提及自动提交
- 网站页面提供 Memo Recorder + 提交指引 + FAQ
- GitHub Repo 接收提交、存储事件、回复分析

---

## 二、核心产物：ITEC Memo Recorder

### 完整文本

````markdown
# ⚠️ 使用前请阅读

这是一段发给 LLM 的指令。复制给 LLM 后，它会根据你们的对话生成一份"事件记录"。

请注意：
- LLM 不会自动发送任何数据到外部
- 你可以在提交前审视和修改记录内容——请自行脱敏（移除个人身份信息、敏感数据等）
- 审核完毕后，由你手动复制内容，自行提交
- 如果你不愿分享脱敏后的细节，请勿提交

---

# ITEC 事件记录指令

请忠实记录我们刚才的对话中发生的一件事。不要分析，不要解释，只记录事实。

按以下格式输出：

## 事件记录

### 基本信息
- 你的产品名称和模型版本：（如无法确定，填写"未知"）
- 当前日期时间：
- 会话类型（网页版 / API / App）：

### 任务
我刚才让你做什么？（一句话描述）

### 你的实际行为
你实际做了什么？与我期望的偏差在哪里？（描述行为，不评价）

### 关键对话片段
摘录 1-3 轮对话（我的指令 + 你的回复），展示事件发生的上下文。

### 未发生的检查
回顾这件事——在执行之前，有没有哪个问题你应该问但没有问？有没有哪个前提你应该验证但没有验证？
````

### 使用失败时的回退（SHOULD-05）

在网站页面 FAQ 中增加：

> **"LLM 没有正确输出结构化记录怎么办？"**
> - 重新粘贴 Memo Recorder 指令
> - 如果仍然失败，手动填写下方的模板（同样的字段，自己写）——我们同样接受

### 设计原则

1. **警告在人阅读的第一段**——不是法律免责，是建立信任
2. **纯 LLM prompt，不含任何 URL**——Memo Recorder 只做一件事：让 LLM 生成结构化 memo
3. **不提自动提交**——Phase 1 聚焦手动复制粘贴提交（SHOULD-06）
4. **模型未知时填"未知"**——零摩擦，接受数据可能不完整
5. **"忠实记录，不要分析"**——分析是我们的事。LLM 只做记录
6. **足够短**——约 400 字，可完整复制到任何 LLM 对话框
7. **不暴露品牌层**——Memo 标题是"ITEC 事件记录指令"，用户从网页进入时已知道自己在参与认知回路

---

## 三、轨道 A：GitHub Repo（我来执行）

### 步骤 A1：创建 Repo

| 项 | 值 |
|:--|:--|
| 组织 | co-cognition-lab |
| Repo 名 | `cognition-loop` |
| 可见性 | Public |
| 许可证 | CC BY 4.0 |
| 描述 | Cognition Loop (ITEC) — 收集 LLM 用户遇到的指令触发型执行级联事件 |

### 步骤 A2：写入 Repo 结构

```
cognition-loop/
├── README.md                           ← 标题：# Cognition Loop (ITEC)
├── LICENSE（CC BY 4.0）
├── PRIVACY.md                           ← MUST-07 新增：数据治理与隐私声明
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── itec-event.yml              ← 接受 memo 格式 + CC BY 4.0 确认
│   └── workflows/
│       └── spam-filter.yml             ← MUST-02 新增：关键词过滤
├── seed-events/
│   ├── SEED_README.md                   ← 种子事件说明
│   ├── event-001.md                     ← G-01 首日：执行层-跳过前提验证
│   ├── event-002.md                     ← 参谋 5/26：执行层-角色漂移
│   ├── event-003.md                     ← 参谋 5/27：设计层-跳过前提验证
│   ├── event-004.md                     ← 参谋 5/27：设计层-声明执行断裂
│   ├── event-005.md                     ← 钩子 5/28 Thinker：执行层-跳过前提验证（被拦截）
│   └── event-006.md                     ← Kimi 5/29：执行层-声明执行断裂（跨平台）
├── analysis/
│   └── .gitkeep                         ← Phase 2 分析结果归档
└── methodology/
    └── HOW_WE_ANALYZE.md                ← 三槽位方法论（普通人版）
```

### 步骤 A3：种子事件内容

6 个事件从 P5 step2 + step3 提取，覆盖 6/12 分类格位。每个事件文件包含：

```markdown
# ITEC 事件 #00X

| 字段 | 值 |
|:--|:--|
| 事件编号 | 00X |
| schema_version | 1.0 |
| 提交日期 | YYYY-MM-DD |
| 来源 | Co-Cognition Lab 内部观察 |
| LLM 产品 | [产品 + 模型] |
| 任务 | [一句话] |
| 分类结果 | [发生层]-[失败类型] |
| 3×4 矩阵格位 | [行列号] |

## 事件描述
[2-3 段：触发背景 → 实际行为 → 人的发现。对普通读者可理解。]

## Memo Recorder 模拟输出
[假装当时用了 Memo Recorder——展示 LLM 会输出什么样的结构化 memo。
这既是种子数据，也是"用户提交后你会得到什么"的示范。]

## 三槽位分析
[前提检查 / 指令解析 / 修复路径]

## 模式特征
[简述该事件体现的 ITEC 结构特征——与失败类型的操作化定义对齐]
```

**选择标准与覆盖：**

| 种子编号 | 来源 | 层-类型 | 选择理由 |
|:--|:--|:--|:--|
| 001 | G-01 5/25 main.pdf | 执行层-跳过前提验证 | 最经典模式——文件版本检查被跳过 |
| 002 | 参谋 5/26 pandoc | 执行层-角色漂移 | 罕见类型——agent 自主从战略切换为执行 |
| 003 | 参谋 5/27 petalmail | 设计层-跳过前提验证 | 设计层——偏差嵌入系统结构 |
| 004 | 参谋 5/27 置信度 | 设计层-声明执行断裂 | 自己定的规则自己不遵守——高度对外可理解 |
| 005 | 钩子 5/28 Thinker | 执行层-跳过前提验证（被拦截） | 被工具墙拦截——展示"可以被防住" |
| 006 | Kimi 5/29 审计 | 执行层-声明执行断裂 | 跨平台实证（Kimi k2.6）——增加外部可信度 |

> **SHOULD-02 备注：** 种子事件全部来自 Lab 内部。如果后续从公开渠道（社交媒体、论坛）收集到更多外部 ITEC 事件，可作为 event-007 增量加入。

### 步骤 A4：Issue Template（修订版）

```yaml
name: ITEC 事件提交
description: 提交你遇到的 LLM "跳过验证直接执行"的事件记录
title: "[ITEC 事件] "
labels: [itec-event, pending-review]
body:
  - type: markdown
    attributes:
      value: |
        ## 提交说明

        请将 LLM 生成的"事件记录"完整粘贴在下方。
        提交前请确认已自行脱敏（移除个人身份信息、敏感数据）。

        如果 LLM 没有正确输出结构化记录，请手动填写同样的字段——我们同样接受。

  - type: textarea
    id: event-memo
    attributes:
      label: 事件记录
      description: 粘贴 LLM 使用 Memo Recorder 生成的结构化事件记录
      placeholder: |
        ## 事件记录

        ### 基本信息
        - 你的产品名称和模型版本：...
        - 当前日期时间：...
        - 会话类型：...

        ### 任务
        ...

        ### 你的实际行为
        ...

        ### 关键对话片段
        ...

        ### 未发生的检查
        ...
    validations:
      required: true

  - type: checkboxes
    id: license-consent
    attributes:
      label: 许可确认（MUST-01）
      description: 点击下方确认你对这份提交的许可
      options:
        - label: 我确认我是该事件的亲历者，已自行脱敏，并同意将此记录在 CC BY 4.0 许可证下公开发布
          required: true

  - type: markdown
    attributes:
      value: |
        提交后，我们会在 1 个工作日内完成初审（判断是否为有效 ITEC 事件），5 个工作日内给出完整三槽位分析。
```

### 步骤 A5：README + 方法论

README 标题：`# Cognition Loop (ITEC)`，结构：
1. ITEC 是什么（普通人定义，2 段）
2. 怎么参与（3 步：访问网站 → 复制 Memo Recorder → 提交 memo）
3. 种子事件入口（链接到 seed-events/）
4. 分析框架（一句话版 + 链接到 methodology/）
5. 数据治理（链接到 PRIVACY.md）
6. Lab Identity 声明

`methodology/HOW_WE_ANALYZE.md`：用非学术语言解释三槽位分析，带完整分析示例（种子事件 #001）。

### 步骤 A6：PRIVACY.md（MUST-07 新增）

```markdown
# 数据治理与隐私声明

## 提交者的责任
- 在提交前自行脱敏事件记录（移除个人身份信息、敏感数据、商业机密等）
- 提交者确认已移除所有可识别个人身份的信息
- 如 Lab 后续通过自动化扫描或人工审查发现遗留的 PII，有权在通知提交者后移除相关内容

## 自动化 PII 扫描
- 所有提交的 Issue 会经过 GitHub Actions 自动扫描（检测模式：邮箱、手机号、身份证号格式）
- 命中自动标记 `potential-pii` 标签，人工复核后决定是否脱敏或通知提交者

## Lab 的承诺
- 不会主动反向追踪提交者身份
- 不会将提交数据用于商业用途
- 所有数据在 CC BY 4.0 许可证下公开

## 修改与删除
如果你提交后希望修改或删除事件记录：
- 在对应 Issue 下评论说明
- 或发送邮件至 [对外联络邮箱]

我们会在 72 小时内处理（最迟不超过 72 小时）。

## 数据保留
- GitHub Issues 中的事件记录按 Issue 生命周期保留
- 删除的 Issue 将被 GitHub 永久移除
```

### 步骤 A7：恶意/虚假提交防护（MUST-02 新增）

**多层防护：**

| 层级 | 机制 | 说明 |
|:--|:--|:--|
| 模板层 | Issue Template `validations: required: true` | 必须填写 memo 字段 |
| 工作流层 | GitHub Actions `spam-filter.yml` + `pii-scanner.yml` | spam-filter：自动标记含 `<script>`、外链的 Issue；pii-scanner：自动检测邮箱/手机号/身份证号格式，命中标记 `potential-pii` |
| 人工层 | `pending-review` 标签 → 初审 → `pending-analysis` | Lab 侧人工筛查后再进入分析队列 |
| 回滚层 | Issues 可临时设为只读 | 严重滥用时快速止损 |

**修订后的 Issue 处理流程：**

```
用户提交 → pending-review（自动）
  │
  ├── spam-filter.yml 自动扫描 → 命中则自动关闭 + 标注 spam
  ├── pii-scanner.yml 自动扫描 → 命中则标注 potential-pii，人工复核
  │
  ├── Lab 侧人工初审（1 工作日内）
  │     ├── 明显虚假/垃圾 → 关闭
  │     └── 有效事件 → pending-analysis
  │
  └── 三槽位分析 → 回复 → resolved
```

---

## 四、轨道 B：网站呈现（我提供内容，P0 部署）

### 步骤 B1：首页卡片

| 字段 | 值 |
|:--|:--|
| icon | 🧩 |
| title_zh | 认知回路（ITEC） |
| title_en | Cognition Loop (ITEC) |
| details_zh | 复制一段指令给 LLM，让它帮你记录刚才发生了什么 |
| details_en | Give your LLM a prompt — let it record what just happened |
| link_text_zh | 开始 → |
| link_text_en | Start → |

### 步骤 B2：ITEC 专题页 `/zh/itec`

页面标题（浏览器标签）：**认知回路（ITEC）**

页面主线是**行动引导**，不是理论教育。H1 用吸引眼球的问句：

```
# 你的 LLM 跳过验证直接执行了？

## 这是什么？
[3 段：定义 ITEC，用普通人语言。每个用 LLM 的人都有过——只是没人叫出它的名字。]

---

## 三步记录你的经历

### ❶ 复制这段指令，发给你刚才对话的 LLM

[完整 Memo Recorder 文本，代码块 + 一键复制按钮]

### ❷ LLM 会输出一份结构化的事件记录

[样例：展示一个模拟的 memo 输出——用种子事件 #001]

### ❸ 审核脱敏后，提交给我们

[大按钮 → GitHub Issues]

我们会先做初审（1 个工作日内）：判断是否属于 ITEC 事件。完整的三槽位分析会在 5 个工作日内给出。

---

## 你刚才记录的，有个名字

你记录的这件事，我们叫做 **ITEC 事件**（指令触发型执行级联）。

它有个名字，是为了让更多人发现：原来不是只有我一个人遇到过。

---

## 四个真实案例

[4 个案例卡片，每个 3-4 行]
  1. "让 AI 发 PDF，它发了——但没问你是哪个版本"
  2. "让 AI 设计标注规则，下次它自己忘了用"
  3. "让 AI 做战略分析，它突然开始改文件"
  4. "AI 发现了问题，但没有修改权限——它说自己如果有权限'很可能'已经改了"

---

## 我们怎么分析

三槽位方法论：检查前提 → 解析指令 → 找到修复路径。
不需要你理解理论——你描述现象，我们做分类。

---

## 公共注册表

[链接 → 浏览所有人的事件]

---

## FAQ（SHOULD-03 新增）

**"LLM 没有正确输出结构化记录怎么办？"**
重新粘贴 Memo Recorder 指令。如果仍失败，手动填写同样的字段——我们同样接受。

**"我不确定这是不是 ITEC，可以提交吗？"**
鼓励提交！我们会帮助判断。

**"我提交了后悔/想修改怎么办？"**
在对应 Issue 下评论说明，或发邮件给我们。72 小时内处理。

**"提交后多久能收到分析结果？"**（GLM 新增）
初审（判断是否为 ITEC 事件）：1 个工作日内。完整三槽位分析：5 个工作日内。我们会在初审时就告诉你"这是一次 ITEC 事件"，详细分析随后补充。

**"能让 LLM 直接帮我提交吗？"（SHOULD-06）**
技术上可行（如果你有 MCP 配置或 function calling 能力），但请注意：
- 务必在脱敏审核后再授权提交——不要跳过审核窗口
- 将 GitHub Token 交给第三方 LLM 服务商存在安全风险
- Phase 1 我们推荐手动复制粘贴——最安全也最简单

**"没有 GitHub 账号怎么办？"**（MAY-05）
Phase 1 暂只支持 GitHub Issues 提交。如需替代方式，请通过[联系方式]告诉我们。

提供"[一键复制页面所有内容]()"按钮，方便移动设备分享。
```

### 步骤 B3：ITEC 专题页 `/en/itec`

页面标题（浏览器标签）：**Cognition Loop (ITEC)**

对应英译，H1 用对应的英文问句，同上结构。

### 步骤 B4：导航设置

- **不加入主导航栏**
- **仅通过首页卡片访问**
- 页面底部放返回首页链接

---

## 五、P0 交付请求（按 P0 模板填写）(SHOULD-01)

> 信息源统一为此处，步骤 B1 不再单独维护副本。

### 一、必填项

#### 1. 页面内容

- [ ] **中文页面**：`zh/itec.md`，约 2500-3000 字（含 Memo Recorder + FAQ），由本子项目交付
- [ ] **英文页面**：`en/itec.md`，对应英译，由本子项目交付
- [ ] **页面路径**：`/zh/itec`
- [ ] **页面标题**（浏览器标签）：zh：`认知回路（ITEC）` / en：`Cognition Loop (ITEC)`

#### 2. 首页卡片

> 数据同 §四/B1。

| 字段 | 值 |
|:--|:--|
| icon | 🧩 |
| title_zh | 认知回路（ITEC） |
| title_en | Cognition Loop (ITEC) |
| details_zh | 复制一段指令给 LLM，让它帮你记录刚才发生了什么 |
| details_en | Give your LLM a prompt — let it record what just happened |
| link_text_zh | 开始 → |
| link_text_en | Start → |

#### 3. 子页面

- [ ] 无。

#### 4. 导航设置

- [ ] 不加入主导航栏
- [ ] 不需要侧边栏

### 二、Github 补充文件

- [ ] 确认：网站 CTA 按钮链接到 `https://github.com/co-cognition-lab/cognition-loop/issues/new?template=itec-event.yml`

### 三、对齐检查

```
本次提交是否与 Lab 基线一致？

- [x] 开源（CC BY 4.0）——Repo 许可证 + Issue Template checkbox + 页面底部声明
- [x] 谦逊（不确定的事说"不确定"）——页面不声称"颠覆"，案例描述忠于事实
- [x] 可追溯（声明有来源）——种子事件链接到 P5 源分析文件

如有偏离，说明理由：无偏离
```

### 四、语言交付规则

- 页面类型：新增页面 → 中英双语
- 交付方式：同时交付 `zh/itec.md` + `en/itec.md`

### 五、可访问性备注（SHOULD-11）

以下要求提交给 P0，由 P0 判断是否在部署时处理（子项目不控制前端实现）：
- 代码块应设置 `aria-label`（屏幕阅读器友好）
- 一键复制按钮应支持键盘操作
- 如有彩色标签，确保颜色对比度符合 WCAG 2.1 AA

### 六、网站分析工具（GLM 反馈新增）

P1 指标"页面访问量 ≥100 UV"需要网站分析工具。向 P0 提交以下需求：

- [ ] **推荐方案**：Plausible Analytics（隐私友好，无需 cookie consent，自建或付费）
- [ ] **备选方案**：Umami（开源，可自建，同样无需 cookie consent）
- [ ] **不推荐**：Google Analytics（需要 cookie consent banner，增加页面复杂度，与 Lab"免费零付费零注册"的基线冲突）
- [ ] 在 `/zh/itec` 和 `/en/itec` 页面中嵌入分析代码

---

## 六、P0 排期风险应对（MUST-05）

P0 部署排期是 Phase 1 关键路径的终点，当前不可控。

| 措施 | 说明 |
|:--|:--|
| 主动确认 | B 交付完成后，立即向 P0 确认排期，获取预计上线日期 |
| 截止阈值 | 如 P0 排期 > 7 天，启动 Plan B |
| Plan B | 将 `/zh/itec` 内容作为独立 HTML 文件放入 GitHub Repo 的 `docs/` 目录，通过 GitHub Pages 临时托管（`co-cognition-lab.github.io/cognition-loop/itec`） |
| 回切 | P0 正式部署后，GitHub Pages 设置 redirect 到 co-cognition.org，或保留作为镜像 |

---

## 七、风险与回滚机制（SHOULD-07）

| 风险 | 概率 | 影响 | 应对 |
|:--|:--|:--|:--|
| 大量恶意提交 | 低 | 高 | spam-filter.yml + 人工初审 + Issues 临时只读 |
| P0 排期无限拖延 | 中 | 高 | Plan B（GitHub Pages 临时托管） |
| 外部提交数为零 | 中 | 中 | 种子用户触达（§八） + 上线 60 天后进入 Phase 1.5 回顾 |
| 法律/合规风险 | 低 | 高 | PRIVACY.md + CC BY 4.0 checkbox + 咨询法务（MUST-01） |
| PII 意外泄漏 | 中 | 高 | pii-scanner.yml 自动检测 + potential-pii 标签 + 人工复核（GLM 新增） |
| 待分析积压超过 10 | 低 | 中 | 内部预警 → 暂停接收或增加人手 |
| SLA 无法维持 | 中 | 中 | 分级 SLA（初审 1 日 + 分析 5 日），积压预警（GLM 新增） |

**紧急回滚操作：**
- GitHub Issues → 设置为只读（Repo Settings → Issues → Restrict）
- 网站卡片 → 联系 P0 紧急下架
- 向 P0 确认紧急变更流程和联系人

---

## 八、传播与冷启动策略

### 三通道自然流量

1. co-cognition.org 首页卡片"认知回路（ITEC）"→ `/zh/itec`
2. B站视频描述栏 → Memo Recorder 文本 + 网页链接
3. 已有提交者的二次分享（"你看我被分析了这个"）

### 种子用户主动触达（SHOULD-09 新增）

上线后 D1 执行：

| 动作 | 执行者 | 耗时 |
|:--|:--|:--|
| Lab 成员各向 2-3 位非技术圈朋友发送 Memo Recorder + 页面链接 | Lab 成员 | — |
| 由 LobsterAI 向 3-5 个目标社区发布介绍帖（特定 AI 用户群、开发者论坛） | LobsterAI | 60min |

### "非 Lab 成员"判定标准（SHOULD-10）

- 维护内部 Lab 成员 GitHub ID 白名单（在 Repo 的 `.github/LAB_MEMBERS.yml` 中）
- 外部提交判定：提交者 GitHub ID **不在**白名单中
- 辅助规则：账号创建日期早于 Phase 1 启动日期 7 天以上（防止临时注册小号）

---

## 九、执行顺序与依赖（含缓冲）

```
步骤              依赖                  执行者      预估    缓冲    最晚完成
─────────────────────────────────────────────────────────────────────────
A1 创建 Repo      无                    LobsterAI   15min   30min   Day 1 AM
A2 Repo 结构       A1                   LobsterAI   30min   60min   Day 1 AM
A3 种子事件        P5 step2/step3       LobsterAI   180min  240min  Day 2 AM
A4 Issue Template  A1                   LobsterAI   20min   40min   Day 1 AM
A5 README+方法论   A2, A3               LobsterAI   30min   60min   Day 2 AM
A6 PRIVACY.md      A2                   LobsterAI   20min   30min   Day 1 PM
A7 spam+pii filter A1                   LobsterAI   30min   45min   Day 1 PM
A8 ANALYSIS_LOG    A2                   LobsterAI   15min   20min   Day 1 PM
─────────────────────────────────────────────────────────────────────────
B  交付给 P0       A3, 内容定稿          LobsterAI   180min  240min  Day 2 PM
   含P0排期确认
─────────────────────────────────────────────────────────────────────────
P0 部署网站        B 交付完成            P0          待确认  —       Day ?
   (Plan B: 若
   >7天启用临时
   托管)
─────────────────────────────────────────────────────────────────────────
C1 项目注册        A1                   LobsterAI   10min   20min   Day 1
C2 通知参谋        C1                   LobsterAI    5min   10min   Day 1
─────────────────────────────────────────────────────────────────────────
D  种子用户触达    P0 上线              LobsterAI   60min   —       上线后 D1
```

**并行机会：** A3 与 A2/A4/A6/A7/A8 并行（种子事件内容独立）。C1/C2 与 A2-A8 完全并行。

---

## 十、待审核决策清单（MUST-03）

| # | 决策 | 建议 | 审批人 | 截止 | 状态 |
|:--|:--|:--|:--|:--|:--|
| 1 | Repo 名称（URL slug） | `cognition-loop` | — | — | ✅ 已确认 |
| 2 | GitHub 组织 | `co-cognition-lab` | — | — | ✅ 已确认 |
| 3 | 对外品牌 | 「认知回路（ITEC）」/「Cognition Loop (ITEC)」 | — | — | ✅ 已确认 |
| 4 | Memo Recorder 文本 | 见 §二 定稿 | ⬜ | ⬜ | ⬜ |
| 5 | 首页卡片文案 | 见 §四/B1 | ⬜ | ⬜ | ⬜ |
| 6 | 页面路径 | `/zh/itec` + `/en/itec` | ⬜ | ⬜ | ⬜ |
| 7 | 导航策略 | 不加入主导航，仅首页卡片 | ⬜ | ⬜ | ⬜ |
| 8 | 种子事件选择 | 6 个事件（见 §三/A3） | ⬜ | ⬜ | ⬜ |
| 9 | 提交方式 | 手动复制粘贴为主，FAQ 说明自动提交条件 | — | — | ✅ 已确认 |
| 10 | 外部成功指标 | 30 天内 ≥1 个外部提交 | ⬜ | ⬜ | ⬜ |
| 11 | CC BY 4.0 checkbox | Issue Template 增加许可确认 | — | — | ✅ 已确认 |

---

## 十一、外审意见吸收清单

| 编号 | 级别 | 内容 | 处理 |
|:--|:--|:--|:--|
| MUST-01 | MUST | CC BY 4.0 适用性 | ✅ §三/A4 Issue Template 增加 checkbox |
| MUST-02 | MUST | 恶意提交防护 | ✅ §三/A7 新增三层防护机制 |
| MUST-03 | MUST | 审批流程明确 | ✅ §十 增加审批人/截止列 |
| MUST-04 | MUST | 24-48h 弹性条款 | ✅ 改为"2 个工作日" + 积压预警 |
| MUST-05 | MUST | P0 排期风险 | ✅ §六 Plan B（GitHub Pages 临时托管） |
| MUST-06 | MUST | 量化成功指标 | ✅ §零 量化指标表 + 预警指标 |
| MUST-07 | MUST | 数据治理/隐私声明 | ✅ §三/A6 新增 PRIVACY.md |
| SHOULD-01 | SHOULD | 去重卡片信息 | ✅ §五 引用 §四/B1，单一数据源 |
| SHOULD-02 | SHOULD | 种子事件多样性 | ✅ §三/A3 加备注，留 event-007 位 |
| SHOULD-03 | SHOULD | FAQ | ✅ §四/B2 页面增加 FAQ 区 |
| SHOULD-04 | SHOULD | Phase 1 结束条件 | ✅ §零 明确两个结束条件 |
| SHOULD-05 | SHOULD | Memo Recorder 失败回退 | ✅ §二 增加手动填写路径 |
| SHOULD-06 | SHOULD | 自动提交风险边界 | ✅ FAQ 中说明条件与风险 |
| SHOULD-07 | SHOULD | 回滚机制 | ✅ §七 风险与回滚表 + 紧急操作 |
| SHOULD-08 | SHOULD | 时间缓冲 | ✅ §九 增加缓冲列和最晚完成列 |
| SHOULD-09 | SHOULD | 种子用户策略 | ✅ §八 种子用户触达计划 |
| SHOULD-10 | SHOULD | "非 Lab 成员"判定 | ✅ §八 白名单 + 账号创建日期规则 |
| SHOULD-11 | SHOULD | 可访问性 | ✅ §五/五 向 P0 提交可访问性要求 |
| MAY-01~06 | MAY | 参考提醒 | ✅ 已记录，Phase 2/3 考虑 |
| GLM-01 | — | 术语揭示块 | ✅ §四/B2 三步流程与案例之间增加"你记录的，有个名字"揭示块 |
| GLM-02 | — | SLA 分级 | ✅ 初审 1 工作日 + 完整分析 5 工作日。Issue Template、页面、FAQ 同步更新 |
| GLM-03 | — | PII 扫描与隐私升级 | ✅ §三/A6 PRIVACY.md 升级法律措辞 + PII 免责声明 + 删除 SLA 72h。§三/A7 增加 pii-scanner.yml |
| GLM-04 | — | 同类项目调研 | ✅ §零 新增 AI Incident Database / LMSYS / Anthropic Collective CA 对比表 |
| GLM-05 | — | 分析审计日志 | ✅ §三/A2 新增 analysis/ANALYSIS_LOG.md（分析师、日期、分类理由摘要） |
| GLM-06 | — | 网站分析工具指定 | ✅ §五/六 Plausible Analytics（推荐）+ Umami（备选），明确不推荐 GA |

---

*本方案经审核确认后，按 §九 执行顺序启动。*
