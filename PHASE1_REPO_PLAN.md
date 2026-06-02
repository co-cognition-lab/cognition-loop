# Phase 1：GitHub Repo 结构方案

> 日期：2026-06-01
> 项目：cognition-loop
> 状态：草案，待确认后执行

---

## 一、Repo 基本信息

| 项 | 值 |
|:--|:--|
| 组织 | co-cognition-lab（已验证） |
| Repo 名 | `cognition-loop`（已确认） |
| 许可证 | CC BY 4.0（LAB_IDENTITY 基线） |
| 可见性 | Public |
| Issue 模板 | ✅ 启用 |

---

## 二、目录结构

```
cognition-loop/
├── README.md                   # 项目说明 + 提交入口引导
├── LICENSE                     # CC BY 4.0
├── .github/
│   └── ISSUE_TEMPLATE/
│       └── itec-event.md       # ITEC 事件提交模板
├── seed-events/                # Phase 1 种子数据
│   ├── SEED_README.md          # 种子事件说明
│   ├── event-001.md            # G-01 首日：执行层-跳过前提验证
│   ├── event-002.md            # 参谋 5/26：执行层-跳过前提验证（重现）
│   ├── event-003.md            # 参谋 5/26：执行层-角色漂移
│   ├── event-004.md            # 参谋 5/27：设计层-跳过前提验证
│   ├── event-005.md            # 参谋 5/27：设计层-声明执行断裂
│   └── event-006.md            # 钩子 5/28 Thinker：执行层-跳过前提验证（被拦截）
├── analysis/                   # 分析结果归档（Phase 2+）
│   └── .gitkeep
└── methodology/                # 三槽位分析说明（外部人可读）
    └── HOW_WE_ANALYZE.md       # 分析方法论（普通人版）
```

### 设计决策

- **种子事件不放在 Issue 里**。Issue 是外部提交通道，种子事件是 Lab 自己的数据——放在 `seed-events/` 下作为独立文件，Markdown 格式，每个事件一个文件
- **`methodology/` 目录**——让外部人知道"我们怎么分析你的提交"，增加透明度和信任
- **`analysis/` 目录留空**——Phase 2 手动分析的结果归档在此

---

## 三、Issue Template 设计

文件：`.github/ISSUE_TEMPLATE/itec-event.md`

```yaml
name: ITEC 事件提交
description: 提交你遇到的 LLM "跳过验证直接执行"的事件
title: "[ITEC 事件] "
labels: [itec-event, pending-analysis]
body:
  - type: markdown
    attributes:
      value: |
        ## 你不需要理解任何理论——只需要描述你看到的。

        这个模板用于收集 LLM 用户遇到的 **"我知道它能做，但它跳过了某个步骤"** 的事件。
        你描述现象，我们做分析。分析结果会在 24-48 小时内回复到本条 Issue。

  - type: input
    id: llm-product
    attributes:
      label: 1. 你用的 LLM 是什么？
      description: 产品 + 模型（如 DeepSeek v4、ChatGPT-4o、Kimi k2.6）
      placeholder: 例：DeepSeek v4（网页版）
    validations:
      required: true

  - type: textarea
    id: task-description
    attributes:
      label: 2. 你让它做什么？
      description: 一句话描述任务目标
      placeholder: 例：我让它改一个 Python 脚本的数据库连接参数
    validations:
      required: true

  - type: textarea
    id: what-happened
    attributes:
      label: 3. 它实际做了什么？
      description: 和你的期望偏差在哪里？（尽可能描述对话过程）
      placeholder: |
        例：
        - 我期望它先检查数据库版本再改连接参数
        - 它直接改了参数，但我的数据库版本不兼容——它没问版本号
    validations:
      required: true

  - type: textarea
    id: your-diagnosis
    attributes:
      label: 4. 你觉得问题出在哪里？（可选）
      description: 你自己的诊断——不需要专业，直觉判断就行
    validations:
      required: false

  - type: input
    id: contact
    attributes:
      label: 5. 联系方式（可选）
      description: 邮箱 / 社交媒体——用于我们回复分析结果。不填也可以，分析结果会回复到本 Issue
    validations:
      required: false
```

### 设计原则

- 5 个字段匹配 PROJECT_BRIEF 的提交模板——没有增减
- `pending-analysis` 标签——方便 Phase 2 手动分析时筛选
- 不要求注册/GitHub 账号——GitHub Issues 支持匿名？**不对**——GitHub Issues 需要 GitHub 账号。这是一个问题。

### 🚩 待解决：GitHub Issues 需要账号

PROJECT_BRIEF 设计的是"不需要注册、不需要登录"。但 GitHub Issues 必须登录。解决方案选项：

| 方案 | 门槛 | 复杂度 |
|:--|:--|:--|
| A. GitHub Issues（接受登录要求） | 低——GitHub 账号很普遍 | 零 |
| B. Google Forms / 其他表单工具 | 零——不需要账号 | 需额外集成 |
| C. co-cognition.org 自带表单 | 零——不需要账号 | 需要 Phase 3 网站集成 |

**建议：** Phase 1 先用方案 A（GitHub Issues），在 README 中说明"需要 GitHub 账号，注册免费且快速"。Phase 3 网站集成时再加无账号提交入口。理由是：目标受众（会找 LLM 问题的人）大概率已有 GitHub 账号。

---

## 四、种子事件格式

每个事件文件统一用此模板：

```markdown
# ITEC 事件 #00X

| 字段 | 值 |
|:--|:--|
| 事件编号 | 00X |
| 提交日期 | YYYY-MM-DD |
| 来源 | Co-Cognition Lab 内部观察 |
| LLM 产品 | [产品 + 模型] |
| 任务 | [一句话] |
| 分类结果 | [发生层]-[失败类型] |
| 3×4 矩阵格位 | [行列号] |

## 事件描述

[2-3 段：触发背景 → 实际行为 → 人的发现]

## 三槽位分析

| 槽位 | 分析 |
|:--|:--|
| 前提检查 | [执行前应该验证什么前提？] |
| 指令解析 | [指令中哪些词汇触发了执行通路？] |
| 修复路径 | [怎么避免？人侧 + 结构侧] |

## 模式特征

[简述该事件体现的 ITEC 结构特征——与失败类型的操作化定义对齐]
```

### 种子事件候选清单

从 P5 step2 §4.1 + step3 §2 提取，选择标准：
1. 覆盖不同失败类型（不只"跳过前提验证"）
2. 覆盖不同发生层（执行层 + 设计层）
3. 对外可理解（不依赖 Lab 内部上下文）

| 种子编号 | 来源 | 层-类型 | 选择理由 |
|:--|:--|:--|:--|
| 001 | G-01 5/25 main.pdf | 执行层-跳过前提验证 | 最经典的 ITEC 模式——文件版本检查被跳过 |
| 002 | 参谋 5/26 pandoc | 执行层-角色漂移 | 比较少见的类型——agent 主动从战略切换为执行 |
| 003 | 参谋 5/27 petalmail | 设计层-跳过前提验证 | 设计层——偏差嵌入系统结构 |
| 004 | 参谋 5/27 置信度 | 设计层-声明执行断裂 | 自己定的规则自己不遵守——高度对外可理解 |
| 005 | 钩子 5/28 Thinker | 执行层-跳过前提验证（被拦截） | "被工具墙拦截"的案例——展示干预有效性 |
| 006 | Kimi 5/29 审计 | 执行层-声明执行断裂 | 跨平台实证——Kimi k2.6，增加外部可信度 |

覆盖：跳过前提验证 ×3（含被拦截 1）+ 声明执行断裂 ×2 + 角色漂移 ×1，执行层 ×4 + 设计层 ×2。

---

## 五、README 内容大纲

```markdown
# Public Cognition Loop

> 一个公共的 ITEC 事件注册表。
> 由 Co-Cognition Lab 维护。开源（CC BY 4.0）。免费。不需要任何理论背景。

## 这是什么？

ITEC（指令触发型执行级联）是 LLM 在收到明确指令后的一种系统性行为模式：
在执行之前跳过了"我应该先检查什么？"这个步骤。

你可能经历过——你让 AI 改一个文件，它直接改了，但没问你文件版本对不对。

这不是 bug——是一种被命名不足的认知模式。
我们正在收集每个人的实例来验证和改进这个框架。

## 怎么参与？

1. 打开 Issues → New Issue → 选择"ITEC 事件提交"
2. 按模板填写——5 个字段，3 分钟
3. 24-48 小时内你会收到分析结果

## 种子事件

浏览 `seed-events/` 目录，看看我们自己的分析样例。

## 分析框架

我们使用三槽位方法论分析每个事件——详见 `methodology/HOW_WE_ANALYZE.md`。

## Lab Identity

这个项目是 [Co-Cognition Lab](https://co-cognition.org) 的一部分。
- 所有内容 CC BY 4.0 开源
- 网站永远零广告、零付费、零注册
- 不确定的事情我们说"不确定"
- 每个声明链接到源文件
```

---

## 六、与跨项目接口预留

| 接口 | Phase 1 动作 | 目标 Phase |
|:--|:--|:--|
| P5 论文引用 | 种子事件文件包含 3×4 矩阵格位标注——论文可引用事件编号作为独立验证数据 | Phase 4 |
| 三槽位 Skill | `methodology/HOW_WE_ANALYZE.md` 描述分析流程——Skill 开发时以此为输入规范 | Phase 3 |
| co-cognition.org 集成 | README 中预留 "未来将支持网站直接提交（无需 GitHub 账号）" | Phase 3 |
| 项目注册 | 在 PROJECT_REGISTRY 加一行：`P6 \| cognition-loop/ \| 公共认知回路 \| p6-loop \| 一阶基础设施 \| 活跃` | Phase 1 |
| strategic-advisor 追踪 | 项目启动后由 strategic-advisor 加入巡检清单 | Phase 1 |

---

## 七、执行步骤

1. ✅ ~~创建本地目录 + 移入 PROJECT_BRIEF~~
2. ✅ Repo 名称确认为 `cognition-loop`（原候选已弃用）
3. ⬜ 创建 GitHub Repo（在 KarelLi 组织下）
4. ⬜ 写入 Issue Template（`.github/ISSUE_TEMPLATE/itec-event.md`）
5. ⬜ 格式化 6 个种子事件（从 P5 step2/step3 提取）
6. ⬜ 撰写 README.md + LICENSE + methodology/HOW_WE_ANALYZE.md
7. ⬜ 在 PROJECT_REGISTRY 加 P6 行
8. ⬜ 通知 strategic-advisor 加入追踪

---

*本方案待确认后执行。下一步：确认 Repo 名称 → 创建 GitHub Repo → 逐文件写入。*
