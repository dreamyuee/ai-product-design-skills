# AI Product Design Skills

Two agent skills I use every day as a product designer working with AI coding agents (Claude Code, Codex). They turn product judgment I used to apply by hand into reusable instructions the agent follows.

> 我是一名产品设计师。这两个 skill 是我在日常用 Claude Code / Codex 做产品时，把自己反复使用的产品判断方法沉淀下来的结果。Skill 正文为中文。

| Skill | What it does | 用途 |
|---|---|---|
| [`check-product-flow`](skills/check-product-flow/SKILL.md) | Walks every step of a flow through 7 checks: entry conditions, confirmed success, skip, exit & resume, failure & retry, duplicate actions, backend records. Outputs rules, acceptance scenarios and open questions. | 逐步骤检查流程的进入条件、提交成功、跳过、退出恢复、失败重试、重复操作和后台记录，补全异常路径。 |
| [`product-thinking`](skills/product-thinking/SKILL.md) | Picks a mode first (diagnose → compare → full spec) so the agent doesn't write a full PRD for an unvalidated problem. Covers user value, business value, cost, feasibility, risk and metrics, plus a Task → Input → Tool → Connector → Agent framework for AI agent products. | 先判断是诊断问题、比较方案还是完整交付；评估价值、成本、风险和验证指标，并提供 Agent 产品的分析框架。 |

## Why these exist

AI agents are great at producing a happy-path spec fast. They are bad at noticing what happens when the user closes the tab after step 1, double-clicks "Submit", or gets charged credits twice on retry. Those gaps used to surface in QA or after launch. These skills make the agent check them **while the design is still cheap to change**.

## Example

Ask: *"Check this two-question onboarding flow."*

Without the skill, you get "save the current step". With `check-product-flow`, the agent separates cases like:

- Closed before submitting Q1 → restore the Q1 draft, **not** recorded as skipped
- Q1 saved on the server but the response was lost → query state on return; don't ask again or grant the reward twice
- Double-click or two tabs → the business action runs once; repeats return the existing result

## Install

**Claude Code**

```bash
git clone https://github.com/dreamyuee/ai-product-design-skills.git
cp -R ai-product-design-skills/skills/* ~/.claude/skills/
```

**Codex**: copy the skill folders into your Codex skills directory. `agents/openai.yaml` holds the display metadata.

Then just describe your product problem. The skills trigger on requests like "review this PRD", "should we build this", or "check the flow".

## How they work together

`product-thinking` decides *what* to build and why. When the answer involves a flow, it hands off to `check-product-flow` for the step-by-step edge cases. They're kept separate on purpose, so each stays short and focused.

## License

MIT
