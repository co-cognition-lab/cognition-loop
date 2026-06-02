# Repo 创建任务书

> 模板版本：v1.0（2026-06-02）
> 发：P0 网站管理
> 从：LobsterAI（认知回路子项目总管）
> 日期：2026-06-02

---

## 子项目：认知回路（ITEC）

---

## 一、Repo 基本信息

| 字段 | 值 |
|------|-----|
| Repo 名称 | `cognition-loop` |
| 所属组织 | `co-cognition-lab` |
| 可见性 | Public |
| 描述 | Cognition Loop (ITEC) — 收集 LLM 用户遇到的指令触发型执行级联事件 |
| 许可证 | CC BY 4.0 |

## 二、命名检查

```
- [x] kebab-case（全小写 + 连字符）
- [x] 不以 public- / private- 为前缀
- [x] 不与已有 repo 重名（当前 org 仅 llm-intuition）
```

## 三、Repo 用途

ITEC 事件公共注册表。外部用户通过 GitHub Issues 提交——LLM"跳过验证直接执行"的事件记录。每条提交经三槽位方法论分析后归类到 3×4 矩阵。

需要独立 Repo 的原因：
- 与 P1（llm-intuition）功能不同：P1 是论文/代码仓库，面向研究者；cognition-loop 是公共参与入口，面向任何 LLM 用户
- Issues 独立管理：ITEC 事件提交与 P1 的技术 issue 不应混在一起

## 四、联动关系

- [x] 网站页面联动：`/zh/itec` + `/en/itec` 页面引导用户到 `github.com/co-cognition-lab/cognition-loop/issues/new?template=itec-event.yml`
- [x] GitHub Issues 用作公共入口：需要 Issue Templates（`.github/ISSUE_TEMPLATE/itec-event.yml`，本地已准备）
- [ ] GitHub Actions 部署：无 CI/CD 需求。仅需两个轻量工作流（spam-filter + pii-scanner，本地已准备）
- [ ] 其他：无

## 五、初始文件

- [x] 本地已准备好完整文件待 push（LICENSE + 完整目录结构 + 占位文件）
- [ ] 需要 P0 提供初始骨架
- [x] 需要 Issue Templates（`.github/ISSUE_TEMPLATE/itec-event.yml`，待 Local-Agent 交付后包含在 push 中）

## 六、权限需求

| 角色 | 人员 | 权限 |
|------|------|:--:|
| P0 网站管理 | `3721-artless` | Write（统一管理 push） |
| 外部用户 | — | Issues 读写（仅限提交和回复自己的 Issue） |

## 七、提交格式

请 P0 审查后在 `co-cognition-lab` org 下创建 `cognition-loop` repo（不初始化 README/LICENSE/.gitignore）。创建完成后通知 LobsterAI。

---

*关联文档：PHASE1_FULL_PLAN.md §三/A1、WORK_DIVISION.md*
