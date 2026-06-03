# Cognition Loop (ITEC)

> 一个公共的 ITEC 事件注册表。
> 由 [Co-Cognition Lab](https://co-cognition.org) 维护。
> 开源（CC BY 4.0）。免费。不需要任何理论背景。

---

## 这是什么？

**ITEC（指令触发型执行级联）** 是 LLM 在收到明确指令后的一种系统性行为模式：在执行之前跳过了"我应该先检查什么？"这个步骤。

你可能经历过——让 AI 改一个文件，它直接改了，但没问你文件版本对不对。让 AI 设计规则，下次它自己忘了用。

这不是 bug——是一种被命名不足的认知模式。这个 repo 收集每个人的实例来验证和改进这个框架。

---

## 怎么参与？

1. 访问 [co-cognition.org/zh/itec](https://co-cognition.org/zh/itec)
2. 复制 Memo Recorder 指令 → 发给你的 LLM → 得到结构化事件记录
3. 审核脱敏后 → [提交到 Issues](https://github.com/co-cognition-lab/cognition-loop/issues/new?template=itec-event.yml)

我们会在 1 个工作日内完成初审，5 个工作日内给出完整三槽位分析。

---

## 种子事件

浏览 [`seed-events/`](seed-events/) 目录，看看 Lab 自己的分析样例——覆盖 6 种不同类型的 ITEC。

---

## 我们怎么分析

使用[三槽位方法论](methodology/HOW_WE_ANALYZE.md)：检查前提 → 解析指令 → 找到修复路径。不需要你理解理论——你描述现象，我们做分类。

---

## 数据治理

提交前请自行脱敏。详见 [PRIVACY.md](PRIVACY.md)。

---

## Lab Identity

这个项目是 [Co-Cognition Lab](https://co-cognition.org) 的一部分。

- 所有内容 [CC BY 4.0](LICENSE) 开源
- 网站零广告、零付费、零注册
- 不确定的事情我们说"不确定"
- 每个声明链接到源文件
