# SKILL_INDEX · 本地 Skill 总注册表

> 用途：`SKILL_ROUTER.md` 的查询底表；每个 skill 只登记元数据与触发条件，不复制 skill 正文。
> 更新规则：新增/删除/改名 skill 时先更本表，再更 router。
> 创建：2026-09-01 ｜ 来源：`D:\civ-mind-sim\federation\SKILL_ROUTER_BUILD_PLAN.md`

## 使用方式

1. DSH 每次任务先读 `D:\skill-registry\SKILL_ROUTER.md`。
2. Router 按任务类型在本表检索匹配项。
3. 单轮最多激活 2-3 个 skill；冲突项不并存。
4. 本表只做分流，不替代知识库 `00_INDEX.md` 的知识权威。

---

## A. 知识库虚拟 Skill（本地规则，非下载仓库）

```yaml
name: obsidian-vault-reader
category: knowledge-base
path: D:\game-design-skills\_鲧禹知识库\
entry: 00_INDEX.md
triggers: [检索资料库, 查世界观, 查剧情, 查机制, 查数值, 读Obsidian笔记]
conflicts: [obsidian-vault-writer, engine-code-writer]
requires: [read-only]
summary: 只读检索知识库；先读 00_INDEX，按需打开对应分类文件，不全文常驻。
```

```yaml
name: obsidian-vault-writer
category: knowledge-base
path: D:\game-design-skills\_鲧禹知识库\
entry: 00_INDEX.md
triggers: [写Steam文案, 写短视频脚本, 写社区话题, 新增知识, 更新索引, 长输出落盘]
conflicts: [obsidian-vault-reader, engine-code-writer]
requires: [workspace-write, 先读00_INDEX, 先更新索引再写正文]
summary: 向知识库写入/更新 Markdown；每条知识带来源；冲突时按 00_INDEX 权威顺序裁决。
```

```yaml
name: engine-code-writer
category: forbidden
path: D:\civ-mind-sim\
triggers: [写游戏引擎代码, 落地机制代码, 修改engine.py]
conflicts: [obsidian-vault-reader, obsidian-vault-writer, all-design-skills]
requires: [GLM施工授权]
summary: 知识库子代理禁止加载；游戏引擎代码施工归 GLM。
```

---

## B. 游戏设计仓库 Skill（D:\game-design-skills，已验货）

```yaml
name: game-architect
category: game-design/numerics
path: D:\game-design-skills\game-architect
entry: SKILL.md
triggers: [数值平衡, 系统机制, 经济曲线, 成长曲线, GDD数值表]
conflicts: [game-design-document-creater]
requires: [read-only]
summary: 数值最深的全流程设计 skill；S-01/S-02 做系统与数值。首选数值设计。
```

```yaml
name: claude-game-design-suite
category: game-design/theory
path: D:\game-design-skills\claude-game-design-suite
entry: game-design-skill-suite.md
triggers: [核心循环, 设计理论, 概念发散, 设计一致性, 全流程框架]
conflicts: [game-design-document-creater]
requires: [read-only]
summary: 22 个 skill 覆盖完整设计生命周期，理论最全；含 Design Coherence Engine 管理变更传播。
```

```yaml
name: gstack-game
category: game-design/workflow
path: D:\game-design-skills\gstack-game
entry: SKILL.md
triggers: [设计评审, 开发流程, 29技能, 立项规划, spark-lens创意发散]
conflicts: [gamestack]
requires: [read-only]
summary: 29 个交互式游戏开发工作流 skill；最强在设计评审与规划。
```

```yaml
name: gamestack
category: game-design/workflow
path: D:\game-design-skills\gamestack
entry: README.md
triggers: [全流程搭架子, 平台无关游戏框架, 工程化游戏开发]
conflicts: [gstack-game]
requires: [read-only]
summary: 类 gstack 的 agentic 游戏开发框架；流程化工程能力。
```

```yaml
name: game-development-orchestrator
category: game-design/orchestration
path: D:\game-design-skills\game-development-orchestrator
entry: SKILL.md
triggers: [多专业编排, 预生产规划, 范围缩减, 跨职能设计对齐]
conflicts: [gstack-game]
requires: [read-only]
summary: 把模糊概念变成可交付开发计划；产出设计文档/平衡表/资产规格。
```

```yaml
name: game-design-skills
category: game-design/knowledge-base
path: D:\game-design-skills\game-design-skills
entry: README.md
triggers: [查设计原理, 查100原理, 查150技巧, 查236技巧, 找方法论依据]
conflicts: [none]
requires: [read-only, 只加载命中的子skill]
summary: 从三本经典书籍提取的可调用知识库；82 个 md，按子目录按需加载。
subskills: [game-design-100-principles, game-design-150-tips, game-design-236-tips]
```

```yaml
name: game-design-document-creater
category: game-design/gdd
path: D:\game-design-skills\game-design-document-creater
entry: SKILL.md
triggers: [中文策划案, 生成GDD, 优化策划案, 按某游戏设计]
conflicts: [game-architect, claude-game-design-suite]
requires: [read-only]
summary: 交互式中文问答生成/优化 GDD；输出可落地方案而非泛泛创意。
```

```yaml
name: game-ui-design-workflow
category: game-design/ui
path: D:\game-design-skills\game-ui-design-workflow
entry: README.md
triggers: [UI策划, UI规范, 页面设计, 组件, 雪碧图, UI交付]
conflicts: [none]
requires: [read-only, 不运行install.sh]
summary: UI 全流程：策划→规范→页面→组件→雪碧图→引擎交付。
```

```yaml
name: game-theorist
category: game-design/strategy
path: D:\game-design-skills\game-theorist
entry: README.md
triggers: [博弈论, 对手建模, 机制评审, 谈判, 竞争策略]
conflicts: [none]
requires: [read-only]
summary: 博弈论六阶段；用于评审与对手建模。
```

```yaml
name: mobile-game-research-toolkit
category: game-design/market-research
path: D:\game-design-skills\mobile-game-research-toolkit
entry: README.md
triggers: [手游市场, 赛道研究, Sensor Tower数据, 市场图表]
conflicts: [none]
requires: [read-only, Sensor Tower需BYOK]
summary: 海外手游研究方法论→取数→作图→飞书交付。
```

```yaml
name: cultivation-world-simulator-reference
category: reference/architecture
path: D:\game-design-skills\cultivation-world-simulator
entry: README.md
triggers: [规则约束AI架构, FastAPI+Vue+PixiJS参考, AI Agent模拟器参考]
conflicts: [engine-code-writer]
requires: [read-only, 只读架构文档不复制运行]
summary: AI Agent 驱动的修仙世界模拟器；商业可行性活证据 + 架构参考，不是可加载设计方法论。
```

```yaml
name: ancient-china-setting-pack
category: setting/world
path: D:\game-design-skills\_上古华夏设定包
entry: README.md
triggers: [四部族, 上古华夏设定, 鲧禹世界观, 渠道调研原稿]
conflicts: [none]
requires: [read-only]
summary: 世界观、四部族、鲧禹剧情、沙盒机制映射与渠道调研；权威口径以知识库 00_INDEX 与 HANDOFF 为准。
```

---

## C. 文明沙盒思维包（D:\civ-mind-sim\skills，14 个）

```yaml
name: civ-mind-sim-think-packs
category: simulation/ai-brains
path: D:\civ-mind-sim\skills
triggers: [思维包对战, 沙盒实验, 红队, 盲跑, 禹型/鲧型/桀型/纣型/堡垒流]
conflicts: [engine-code-writer]
requires: [read-only, 对局跑批需另行授权]
summary: 14 个思维包：blind/dsh/extractor/gemini/glm_subagent/gun_overfit/jie/red_team/reformer/single_lens/three_layer/yu_robust/zcode/zhou。
```

```yaml
name: mindharp
category: wellbeing/state-parser
path: D:\mindharp
entry: CANON.md
triggers: [人机协作, 作者状态, 思维投影, T1观测层, 证据分级, 单向阀]
conflicts: [none]
requires: [read-only, 遵守CANON房规, 作者未提交改动不可覆盖]
summary: 大模型力学运行在一个人身上；状态解析器，不是诊断工具。
```

---

## D. 未来工具位（D:\obsidian-vault-skills，尚未下载）

```yaml
name: obsidian-skills
category: tools/obsidian
path: D:\obsidian-vault-skills\obsidian-skills
status: pending-download
triggers: [Obsidian CLI, markdown格式操作]
conflicts: [none]
requires: [先按SECURITY_CHECKLIST检查]
summary: kepano/obsidian-skills；教会 DSH 读写 Obsidian Markdown 的底层 skill。
```

```yaml
name: llm-wiki-agent
category: tools/wiki
path: D:\obsidian-vault-skills\llm-wiki-agent
status: pending-download
triggers: [自维护wiki, 资料导入]
conflicts: [obsidian-vault-writer]
requires: [先按SECURITY_CHECKLIST检查]
summary: SamurAIGPT/llm-wiki-agent；自维护 wiki 主选。
```

```yaml
name: karpathy-llm-wiki
category: tools/wiki
path: D:\obsidian-vault-skills\karpathy-llm-wiki
status: pending-download
triggers: [轻量LLM Wiki, 备选导入]
conflicts: [obsidian-vault-writer]
requires: [先按SECURITY_CHECKLIST检查]
summary: Astro-Han/karpathy-llm-wiki；轻量备选。
```

---

## 冲突规则

- 同一任务不得同时加载两个同类全流程框架（如 gstack-game 与 gamestack）。
- `engine-code-writer` 对知识库子代理永远禁用。
- 检索与写入分离：审计/查证任务加载 reader；产出落盘任务加载 writer。
- 冲突无法避免时，按 `00_INDEX.md` 权威顺序裁决，并把冲突点写入任务回报。
