# DSH Skill Registry · 任务总闸与技能注册表

> **DSH（DeepSeek Harness / 多智能体协作系统）任务路由总闸与技能注册中心。**  
> 解决多 Agent 协作中“技能过多导致上下文冲突、注意力膨胀与 Token 超限”的核心痛点。

---

## 核心设计哲学

在多智能体与复杂业务流水线中，安装大量 Skill 会迅速撑爆大模型的上下文窗口（Context Window），并导致不同工具/规则之间的冲突与幻觉。

本项目采用**按需检索、极简挂载**的架构：
1. **不全量常驻**：绝不将所有 Skill 堆入上下文。
2. **轻量总闸（Router）**：Agent 在承接任务时，优先读取轻量路由表，按任务类型仅激活匹配的 **1~3 个 Skill**。
3. **查表隔离（Index）**：总注册表仅保留元数据、触发词（triggers）、冲突规则（conflicts）与权限要求（requires），不复制 Skill 正文。
4. **越权硬拦截**：对超出席位权责的操作（例如代码施工越权）在路由阶段直接拦截并分流。

---

## 目录结构

```text
dsh-skill-registry/
├── SKILL_ROUTER.md        # DSH 任务总闸：3步判定流程、主路由表与越权拦截规范
├── SKILL_INDEX.md         # 本地 Skill 总注册表：登记各模块技能元数据与权限要求
├── ROUTER_TEST_LOG.md     # 业务与越权分流测试日志（实测通过验证）
├── README.md              # 项目说明
└── LICENSE                # MIT 开源协议
```

---

## 路由工作流

当 DSH 收到任务指令时，遵循**固定三步法**：

```text
[接收任务指令]
       │
       ▼
1. 读取 SKILL_ROUTER.md
       │
       ▼
2. 匹配任务类型（世界观检索 / 机制策划 / 数值平衡 / 业务评审 / 越权拦截）
       │
       ▼
3. 激活匹配项（单轮最多 1 个 Router + 2 个 Skill 摘要 + 1 个知识正文）
   其余 Skill 保持静默，不进入上下文
```

### 上下文装载模板

```markdown
[已加载 router] SKILL_ROUTER.md（本次任务类型：<类型>）
[已加载 skill] <name> —— <SKILL_INDEX.md 中的一行摘要>
[已加载知识] <具体业务文档> —— 仅本次需要的章节
[未加载] 其余 skill 不载入正文
```

---

## 验证与测试记录

在 [ROUTER_TEST_LOG.md](./ROUTER_TEST_LOG.md) 中记录了多场景实测表现：
- **资料检索**：正确挂载 reader 与索引，正文按需打开，未加载多余工具。
- **业务文案**：独立挂载 writer，未与 reader 冲突。
- **核心评审**：挂载 theorist 与评审记录，认知分规范标注。
- **越权拦截**：对代码修改等越权指令，路由层坚决拒绝并分流转交。

---

## 开源协议

本项目基于 [MIT License](./LICENSE) 开源。
