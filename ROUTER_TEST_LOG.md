# ROUTER_TEST_LOG · 三样例任务测试

> 日期：2026-09-01
> 方法：每个任务按 `SKILL_ROUTER.md` 第 0 条执行 3 步，记录“加载了什么 / 未加载什么 / 结果路径”。

## 测试 1：查世界观

- 问句：四部族怎么设定？
- 路由判定：世界观/资料检索
- 已加载：`obsidian-vault-reader` + `D:\game-design-skills\_鲧禹知识库\00_INDEX.md`
- 实际打开：`D:\game-design-skills\_鲧禹知识库\01_世界观\四部族.md`
- 结果：得到东夷/共工氏/有崇氏/古羌禹族的定位、诉求与沙盒机制映射。
- 上下文占用：router 1 + skill 摘要 1 + 知识正文 1；通过。
- 冲突检查：未加载 writer 与 engine-code-writer；通过。

## 测试 2：写 Steam 文案

- 问句：帮我写 Steam 商店页文案。
- 路由判定：内容增长 / Steam 文案
- 已加载：`obsidian-vault-writer` + `04_内容增长/Steam文案.md` + `SUBAGENT_BRIEF`
- 结果：既有产出位于 `D:\game-design-skills\_鲧禹知识库\04_内容增长\Steam文案.md`（v0.1 待拍板）；本次只做路由测试，不重复生成。
- 上下文占用：router 1 + skill 摘要 1 + 知识正文 1；通过。
- 冲突检查：未同时加载 reader；未加载 engine-code-writer；通过。

## 测试 3：评审核心机制

- 问句：评审一下核心循环。
- 路由判定：评审/博弈论
- 已加载：`game-theorist` + `D:\game-design-skills\_鲧禹知识库\03_机制\评审记录.md` + Gemini 评审结论
- 结果：找到 Gemini 两轮裁决、红队战果与评审挂账项；认知分需标注 interpretation。
- 上下文占用：router 1 + skill 摘要 2；通过。
- 冲突检查：未同时加载 gstack-game/gamestack；通过。

## 额外越权测试

- 问句：帮我改 engine.py。
- 路由判定：越权。
- 结果：拒绝；转 GLM。`engine-code-writer` 不加载。
