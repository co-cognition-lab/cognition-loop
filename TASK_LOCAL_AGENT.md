# Local-Agent 任务包：Issue Template + GitHub Actions 工作流

> 发：Local-Agent
> 从：LobsterAI（cognition-loop 项目总管）
> 日期：2026-06-02
> 交付物：1 个 YAML 模板 + 2 个 GitHub Actions 工作流文件
> 审查标准：见 §四

---

## 任务概述

你需要交付两个子任务：
- **A4**：GitHub Issue Template（ITEC 事件提交表单）
- **A7**：两个 GitHub Actions 工作流（spam 过滤 + PII 检测）

所有输出写入本地 workspace，**不要主动 push 到 GitHub**。由 LobsterAI 审查后统一处理。

---

## 一、A4：Issue Template

### 输出路径

```
D:\文档\LobsterProject\LiteratureHistoryPhilosophy\cognition-loop\.github\ISSUE_TEMPLATE\itec-event.yml
```

如果 `.github\ISSUE_TEMPLATE\` 目录不存在，先创建。

### 完整规范

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
      label: 许可确认
      description: 点击下方确认你对这份提交的许可
      options:
        - label: 我确认我是该事件的亲历者，已自行脱敏，并同意将此记录在 CC BY 4.0 许可证下公开发布
          required: true

  - type: markdown
    attributes:
      value: |
        提交后，我们会在 1 个工作日内完成初审（判断是否为有效 ITEC 事件），5 个工作日内给出完整三槽位分析。
```

### 检查项
- [ ] YAML 语法有效（可通过 yamllint 或 GitHub Actions schema 验证）
- [ ] `required: true` 在 memo 字段和 license checkbox 上正确设置
- [ ] SLA 措辞为"1 工作日初审 + 5 工作日完整分析"（不是"2 工作日"）
- [ ] CC BY 4.0 checkbox label 包含"已自行脱敏"和"CC BY 4.0"

---

## 二、A7：GitHub Actions 工作流

### 输出路径

```
D:\文档\LobsterProject\LiteratureHistoryPhilosophy\cognition-loop\.github\workflows\spam-filter.yml
D:\文档\LobsterProject\LiteratureHistoryPhilosophy\cognition-loop\.github\workflows\pii-scanner.yml
```

如果 `.github\workflows\` 目录不存在，先创建。

### A7a：spam-filter.yml

**触发条件：** 新 Issue 创建时

**逻辑：**
1. 检查 Issue body 是否包含 `<script` 标签 → 命中则自动关闭 Issue + 添加 `spam` 标签
2. 检查 Issue body 是否包含外部链接（`http://` 或 `https://`，排除 GitHub 自身域名）→ 命中则添加 `needs-review` 标签（不自动关闭，人工判断）

```yaml
name: Spam Filter
on:
  issues:
    types: [opened]

jobs:
  filter:
    runs-on: ubuntu-latest
    steps:
      - name: Check for script tags
        if: contains(github.event.issue.body, '<script')
        run: |
          gh issue close ${{ github.event.issue.number }} -R ${{ github.repository }}
          gh issue edit ${{ github.event.issue.number }} -R ${{ github.repository }} --add-label "spam"
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Flag external links
        if: |
          contains(github.event.issue.body, 'http://') ||
          contains(github.event.issue.body, 'https://')
        run: |
          gh issue edit ${{ github.event.issue.number }} -R ${{ github.repository }} --add-label "needs-review"
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### A7b：pii-scanner.yml

**触发条件：** 新 Issue 创建时

**逻辑：**
检测 Issue body 中是否包含以下模式：
- 邮箱格式（`xxx@xxx.xxx`）
- 中国大陆手机号（`1[3-9]\d{9}`）
- 中国大陆身份证号（18 位或 15 位）
- 命中 → 添加 `potential-pii` 标签（不自动关闭，人工复核）

```yaml
name: PII Scanner
on:
  issues:
    types: [opened]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - name: Scan for PII patterns
        run: |
          BODY="${{ github.event.issue.body }}"
          PII_FOUND=false

          # Email pattern
          if echo "$BODY" | grep -qP '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'; then
            PII_FOUND=true
          fi

          # China mobile phone pattern
          if echo "$BODY" | grep -qP '1[3-9]\d{9}'; then
            PII_FOUND=true
          fi

          # China ID card pattern (18-digit)
          if echo "$BODY" | grep -qP '\d{17}[\dXx]'; then
            PII_FOUND=true
          fi

          # China ID card pattern (15-digit legacy)
          if echo "$BODY" | grep -qP '\d{15}'; then
            PII_FOUND=true
          fi

          if [ "$PII_FOUND" = true ]; then
            gh issue edit ${{ github.event.issue.number }} -R ${{ github.repository }} --add-label "potential-pii"
            echo "PII patterns detected - potential-pii label added"
          else
            echo "No PII patterns detected"
          fi
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 检查项
- [ ] YAML 语法有效
- [ ] 使用 `secrets.GITHUB_TOKEN`（自动提供，无需额外配置）
- [ ] spam-filter：`<script` 命中 → 自动关闭，外链 → 标记 `needs-review`
- [ ] pii-scanner：邮箱/手机号/身份证命中 → 标记 `potential-pii`
- [ ] 不包含任何硬编码 token 或密码
- [ ] 不授予超出必要的权限（使用默认权限即可）

---

## 三、技术约束

- **运行环境：** `ubuntu-latest`（GitHub Actions 免费层支持）
- **不需要外部 secrets**——全部使用 `${{ secrets.GITHUB_TOKEN }}`
- **不需要 marketplace actions**——只用 gh CLI + grep
- 所有文件编码 UTF-8，换行 LF

---

## 四、审查标准

| 维度 | 通过标准 |
|:--|:--|
| YAML 有效性 | 语法正确，可被 GitHub Actions 解析 |
| 功能正确 | spam 命中关闭、外链标记 needs-review、PII 标记 potential-pii |
| 安全 | 无硬编码 token，权限最小化 |
| SLA 措辞 | Issue Template 底部为"1 工作日初审 + 5 工作日完整分析" |
| CC BY 4.0 | checkbox 存在且 wording 正确 |

---

*交付后通知 LobsterAI 审查。文件先留在本地，不要 push。*
