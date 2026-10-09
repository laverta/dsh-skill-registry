# SKILL_ROUTER · DSH 任务总闸

> 用途：DSH 每次任务先读本文件；按任务类型只激活 1-3 个匹配 skill，不把全部 skill 塞进上下文。
> 查询底表：`D:\skill-registry\SKILL_INDEX.md`
> 创建：2026-09-01 ｜ 来源：`D:\civ-mind-sim\federation\SKILL_ROUTER_BUILD_PLAN.md`

## 第 0 条：每次任务固定 3 步

1. 读本文件 `SKILL_ROUTER.md`。
2. 判断任务类型，在下方路由表中找到匹配行。
3. 只激活匹配的 1-3 个 skill；其余一律不载入正文，只可持有其 `SKILL_INDEX` 摘要。

---

## 一、主路由表

| 任务类型 | 加载组合 | 备注 |
|---|---|---|
| 世界观 / 资料检索 | `obsidian-vault-reader` + `00_INDEX.md` | 先索引后开卷；只读需要的那份文件 |
| 编剧 / 剧情 / 人物 | `SUBAGENT_BRIEF` + 知识库 `02_剧情/` | 以 2026-08-31 14:37 版三幕结构为准 |
| 机制设计 / 核心循环 | `game-architect` + `claude-game-design-suite`（core-loop 相关） | 理论打底，数值用 game-architect |
| 数值平衡 | `game-architect`（S-01/S-02）+ 知识库 `03_机制/数值草案.md` | 37 份报告基线是本地黄金数据 |
| UI 设计 | `game-ui-design-workflow` | 不运行 install.sh；产出设计规范与交付清单 |
| GDD / 中文策划案 | `game-design-document-creater` + `ancient-china-setting-pack` | 上古题材时加载设定包 |
| 内容增长 / Steam 文案 | `obsidian-vault-writer` + 知识库 `04_内容增长/` + `SUBAGENT_BRIEF` | 写完落盘；上下文只留结论 |
| 短视频 / 社区话题 | `obsidian-vault-writer` + 知识库 `04_内容增长/` | 遵守防火墙词表 |
| 评审 / 博弈论 | `game-theorist` + `03_机制/评审记录.md` + Gemini 评审结论 | 认知分标注为 interpretation |
| 市场 / 渠道调研 | `mobile-game-research-toolkit` + `04_内容增长/` 既有调研 | 最终渠道口径以 HANDOFF_20260830 为准 |
| 思维包 / 沙盒实验 | `civ-mind-sim-think-packs` + `03_机制/核心机制_MVP.md` | 跑批需另行授权 |
| 冲突裁决 | `00_INDEX.md` 权威顺序 + `SKILL_INDEX.md` 冲突规则 | 先记录，不擅自改 |

## 二、路由规则

1. **最多 3 个 skill**：超过时只保留最匹配项；其余仅用 `SKILL_INDEX.md` 里的一行摘要。
2. **冲突不并存**：见 `SKILL_INDEX.md` 冲突规则；如 `gstack-game` 与 `gamestack` 不同时加载。
3. **检索与写入分离**：查证任务加载 `obsidian-vault-reader`；产出落盘任务加载 `obsidian-vault-writer`。
4. **禁止加载**：`engine-code-writer` 对知识库子代理永久禁用；不跑第三方 install/curl/npx 脚本。
5. **权威顺序**：冲突时按 `D:\game-design-skills\_鲧禹知识库\00_INDEX.md` 第 0 节裁决。
6. **上下文预算**：单轮最多 1 个 router + 2 个 skill 摘要 + 1 个知识库正文文件；遇到 `CONTEXT_WINDOW_EXCEEDED`，先压缩摘要，再重试。
7. **路径纪律**：回答引用 skill 路径与知识库路径；长输出写文件；对话只留结论。

## 三、上下文装载模板

```
[已加载 router] SKILL_ROUTER.md（本次任务类型：<类型>）
[已加载 skill] <name> —— <SKILL_INDEX.md 中的一行摘要>
[已加载知识] <D:\...\具体文件.md> —— 仅本次需要的章节
[未加载] 其余 skill 不载入正文
```

## 四、快速判定示例

| 用户/DSH 问句 | 判为 | 加载 |
|---|---|---|
| “四部族怎么设定？” | 世界观/资料检索 | obsidian-vault-reader + 00_INDEX |
| “帮我写 Steam 商店页” | 内容增长/Steam 文案 | obsidian-vault-writer + 04_内容增长/Steam文案.md + SUBAGENT_BRIEF |
| “评审一下核心循环” | 评审/博弈论 | game-theorist + 03_机制/评审记录.md |
| “调一下数值曲线” | 数值平衡 | game-architect + 03_机制/数值草案.md |
| “改 engine.py” | 越权 | 拒绝，转 GLM；不加载 engine-code-writer |

## 五、验收自检

- 能回答“我应该加载哪个 skill”，并只加载匹配项。
- 两个冲突 skill 不会同时载入。
- 单轮最多 1 个 router + 2 个 skill 摘要。
- 知识类任务仍先过 `00_INDEX.md` 知识权威。
