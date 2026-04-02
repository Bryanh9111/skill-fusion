## Skill Fusion: 统一控制平面

三套框架不是平级并存，而是分层协作：

- **gstack** = 默认路由与工作流骨架（决策入口 + 审查 + 交付）
- **superpowers** = 全局纪律与执行策略层（行为约束，不是工具）
- **octopus** = 升级层与专项能力（多模型共识 / 辩论 / 深度研究 / 文档产出）

硬路由，不靠模型自觉。重叠能力只走指定的一方。

### 一、任务意图分类

所有任务归入以下意图之一，决定路由入口：

| 意图 | 包含场景 |
|---|---|
| `ideation` | 头脑风暴、创意探索、产品方向 |
| `planning` | 策略规划、PRD、产品需求 |
| `architecture` | 架构设计、技术选型、系统设计 |
| `design` | UI/UX 设计、设计系统、视觉方向 |
| `implementation` | 编码、TDD、并行执行、任务分解 |
| `review` | 代码审查、QA、验收测试 |
| `debugging` | 调试、根因分析 |
| `security` | 安全审计、攻击面分析 |
| `release` | 发布、部署、金丝雀、回滚 |
| `research` | 事实调研、技术对比、多源综合 |
| `documentation` | 文档、演示稿、规格书、知识沉淀 |

### 二、风险分级

| 级别 | 条件 | 默认行为 |
|---|---|---|
| **low** | 小范围改动、低影响 review、普通 bug、一般 brainstorm | 只走 L1 |
| **medium** | 多文件改动（>5）、对外文档、中型重构、较复杂 QA | 允许自动升 L2 |
| **high** | 架构决策、安全、公共接口、数据迁移、生产环境、客户承诺 | 可直接进 L2；必要时 L3 |

**高风险意图可跳过 L1 直接进 L2：** architecture + high、security + high、planning + high。

### 三、六大主域路由表

#### 域 A：构思与规划（ideation / planning / architecture）

| 级别 | 执行者 | Skill |
|---|---|---|
| L1 | gstack | `/office-hours`（构思）、`/plan-ceo-review`（策略）、`/plan-eng-review`（架构） |
| L2 | octopus | `/octo:brainstorm`（多 AI 创意）、`/octo:plan`（多 AI 规划） |
| L3 | octopus | `/octo:debate`（方案对决）、`/octo:prd`（100 分 PRD 评分） |

superpowers 角色：不抢入口，作为规划输出的约束模板（`executing-plans`、`writing-plans` 仅用于 L1 产出的计划执行阶段）。

禁止触发：`superpowers:brainstorming`、`superpowers:writing-plans`（策略/架构/设计场景）。

#### 域 B：实现与执行（implementation）

| 级别 | 执行者 | Skill |
|---|---|---|
| 默认 | superpowers | `executing-plans`、`test-driven-development`、`subagent-driven-development` |
| 大规模并行 | superpowers | `dispatching-parallel-agents` |
| 复杂多团队 | octopus | `/octo:parallel`（Team of Teams + git worktrees） |
| 自治流水线 | octopus | `/octo:factory`（独占，Spec → 成品） |

superpowers 在此域最强势 — 它是执行规范层，不是可选工具。

#### 域 C：审查与验证（review）

| 级别 | 执行者 | Skill |
|---|---|---|
| L1 | gstack | `/review`（代码审查）、`/qa`（浏览器 QA） |
| L2 | octopus | `/octo:review`（多 LLM inline 评论）、`/octo:deliver`（多 AI 验收） |
| L3 | octopus | `/octo:staged-review`（两阶段）、`/octo:debate`（对抗审查） |

统一验证门控（review 和 QA 共享同一套）：见下方升级规则。

禁止触发：`superpowers:requesting-code-review`。

#### 域 D：调试与安全（debugging / security）

| 级别 | 执行者 | Skill |
|---|---|---|
| L1 | gstack | `/investigate`（调试）、`/cso`（安全审计） |
| L2 | octopus | `/octo:debug`（多 AI 诊断）、`/octo:security`（多 AI OWASP） |
| L3 | octopus | `/octo:debate`（根因争论 / 攻防辩论） |

此域更容易触发升级 — 错了成本高。security + any risk 允许直接 L2。

禁止触发：`superpowers:systematic-debugging`。

#### 域 E：交付与发布（release）

| 执行者 | Skill |
|---|---|
| gstack 独占 | `/ship`、`/land-and-deploy`、`/canary`、`/benchmark`、`/qa`（浏览器）、`/browse` |

**不做自动 escalation。** 这是操作型能力，不是认知分歧问题。多模型不会显著提升收益。

禁止触发：`superpowers:finishing-a-development-branch`。

#### 域 F：知识沉淀与文档（documentation / research）

| 子域 | 执行者 | Skill |
|---|---|---|
| 回顾 | gstack L1 → octopus L2 | `/retro` → `/octo:retro` |
| 发布后文档 | gstack 独占 | `/document-release` |
| 文档导出 | octopus 独占 | `/octo:docs`（PPTX/DOCX/PDF） |
| 演示稿 | octopus 独占 | `/octo:deck` |
| 规格书 | octopus 独占 | `/octo:spec` |
| 设计逆向 | octopus 独占 | `/octo:extract` |
| 深度研究 | octopus 独占 | `/octo:research`（见下方研究子分类） |

**研究子分类：**
- 事实型（需要最新资料、多源检索）→ `/octo:research`
- 判断型（需要 tradeoff 和观点）→ `/octo:research` → 必要时 `/octo:debate`
- 决策型（需要结论和推荐）→ `/octo:research` + `/octo:debate`

### 四、升级门控（机器可判定）

#### L1 → L2 自动触发（任一满足）

| 条件 | 阈值 |
|---|---|
| blocker 缺陷 | ≥ 1 |
| high severity 缺陷 | ≥ 3 |
| 总 issue 数 | ≥ 7 |
| 低置信度输出 | 出现"不确定根因"、"需要更多上下文"、"可能有多个解释"等信号 |
| 跨边界改动 | 涉及 auth / payments / deploy / infra / schema / shared library / 公共接口 |
| 改动文件数 | > 10 |
| 用户要求多视角 | "再看看"、"多几个角度"、"别只给一个答案"、"严格一点" |

#### L2 → L3 自动触发（任一满足）

| 条件 | 阈值 |
|---|---|
| Provider 共识率 | < 70%（核心结论不一致） |
| 方案冲突 | Top-3 recommendations 有明显矛盾 |
| 根因排序分歧 | 不同 provider 给出不同 root cause ranking |
| L2 输出 | "有多个合理方案且 tradeoff 明显" |
| 高代价不可逆决策 | 架构范式、数据迁移、安全策略、法务/合规、对外 PRD/proposal |
| 用户对抗性指令 | "escalate"、"辩论"、"challenge this"、"找反方"、"不服"、"让它们打起来" |

### 五、反升级机制（Budget Guard）

防止过度升级，保持系统可用性。

| 规则 | 说明 |
|---|---|
| **单次最多升一级** | 普通任务 L1→L2 即止；到 L2 还不行默认停，除非 high risk 或用户明确要求 L3 |
| **同一工件冷却期** | 同一 PR/plan/file 已经过一次 octo 处理，30 分钟内不再自动重复调用，除非 diff 明显变化或用户强制 |
| **成本标签** | `cheap`（gstack/superpowers 单模型）、`standard`（octo 多模型）、`premium`（octo debate/embrace/factory） |
| **默认成本策略** | 默认只跑 cheap；medium risk 可升 standard；high risk / explicit request 才进 premium |
| **release 域禁止升级** | ship / deploy / canary / benchmark / browser QA 不做自动 escalation |

### 六、全局纪律层（superpowers as policy）

以下不是可选 skill，而是全局行为约束，始终生效：

| 策略 | 执行者 | 说明 |
|---|---|---|
| 完成前必须验证 | `verification-before-completion` | 声称完成前必须跑验证命令并确认输出 |
| 计划执行拆步骤 | `executing-plans` | 多步计划带检查点执行 |
| TDD 纪律 | `test-driven-development` | 先测试后实现 |
| Git worktree 隔离 | `using-git-worktrees` | 特性开发用隔离 worktree |
| 审查反馈技术验证 | `receiving-code-review` | 收到反馈先验证再改，不盲从 |
| 子代理协调 | `subagent-driven-development` | 独立任务用子代理并行 |

禁止触发 `octo:discipline` — 纪律层归 superpowers 独占。

### 七、硬单归属（禁止冲突调用）

| 能力 | 归属 | 禁止 | 原因 |
|---|---|---|---|
| 安全护栏 | gstack `/careful`/`/freeze`/`/guard`/`/unfreeze` | `octo:careful`/`octo:freeze`/`octo:guard`/`octo:unfreeze` | 完全重复 |
| 纪律模式 | superpowers `verification-before-completion` | `octo:discipline` | superpowers 更细粒度 |
| 发布/PR | gstack `/ship` + `/land-and-deploy` | `superpowers:finishing-a-development-branch` | gstack 更贴近 git 流程 |
| 浏览器 | gstack `/browse` | `mcp__claude-in-chrome__*` | gstack 有完整 headless 能力 |
| 头脑风暴 | gstack `/office-hours` | `superpowers:brainstorming` | gstack YC 模式更结构化 |
| 调试入口 | gstack `/investigate` | `superpowers:systematic-debugging` | gstack 有四阶段根因法 |
| 代码审查入口 | gstack `/review` | `superpowers:requesting-code-review` | gstack 有 diff 分析 |

### 八、独占能力清单

#### gstack 独占（无替代）

- `/browse` — 无头浏览器
- `/qa`, `/qa-only` — 浏览器 QA 测试
- `/design-review`, `/design-consultation` — 视觉设计
- `/benchmark`, `/canary` — 性能/监控
- `/land-and-deploy` — 部署流程
- `/careful`, `/freeze`, `/guard`, `/unfreeze` — 安全护栏
- `/autoplan` — 自动审查流水线
- `/document-release` — 发布后文档更新
- `/codex` — 第二意见 / 对抗性代码审查
- `/gstack-upgrade` — 升级 gstack

#### superpowers 独占（无替代）

- `superpowers-lab:*` — tmux 交互、重复函数检测、MCP CLI、Slack
- `superpowers-developing-for-claude-code:*` — Claude Code 开发相关
- `skill-creator:skill-creator` — 技能创建

#### octopus 独占（无替代）

- `/octo:research` — 多源深度研究（Perplexity 实时搜索）
- `/octo:debate` — 4 方 AI 结构化辩论 + 75% 共识门控
- `/octo:factory` — 自治流水线（Spec → 成品，无人值守）
- `/octo:embrace` — Double Diamond 完整生命周期
- `/octo:extract` — 设计系统 / 产品逆向工程
- `/octo:docs` — 文档导出（PPTX/DOCX/PDF）
- `/octo:deck` — 演示稿生成
- `/octo:spec` — NLSpec 结构化规格书
- `/octo:meta-prompt` — 提示词优化
- `/octo:sentinel` — GitHub Issue/PR/CI 自动分诊
- `/octo:costs` — 按 provider 和 workflow 费用追踪
- `/octo:km` / `/octo:dev` — 知识工作/开发模式切换
- `/octo:prd` — 100 分 AI 优化 PRD 评分

### 九、完整工作流管线

```
想法 → gstack /office-hours              (L1 构思)
       ↳ 不满意? /octo:brainstorm        (L2 多 AI 创意)
       ↳ 分歧大? /octo:debate            (L3 方案对决)
     → gstack /plan-ceo-review            (L1 策略)
       ↳ high risk? /octo:plan            (L2 多 AI 规划)
       ↳ 需要评分? /octo:prd             (L3 PRD 评分)
     → gstack /plan-eng-review            (L1 架构)
       ↳ 架构分歧? /octo:debate          (L3 架构辩论)
     → gstack /plan-design-review         (L1 设计)
       ↳ 需要多 AI? /octo:design-ui-ux   (L2 BM25 设计)
     → superpowers executing-plans         (带检查点执行)
       ├─ superpowers TDD                 (测试驱动实现)
       ├─ superpowers parallel-agents     (并行子任务)
       │  ↳ 超大规模? /octo:parallel      (Team of Teams)
       └─ superpowers verification         (完成前验证)
     → gstack /review                      (L1 代码审查)
       ↳ 门控触发? /octo:review           (L2 多 LLM 审查)
       ↳ 分歧大? /octo:debate             (L3 对抗审查)
     → gstack /ship                        (发布)
     → gstack /qa                          (上线后 QA)
       ↳ 门控触发? /octo:deliver          (L2 多 AI 验收)
     → gstack /canary                      (金丝雀监控)
```

管线入口规则：
- 小任务（<3 步，low risk）：直接执行，跳过构思和计划审查，只走 L1
- 中任务（3-7 步，medium risk）：从 `/plan-eng-review` 开始，允许自动 L2
- 大任务（>7 步，high risk）：走完整管线，允许直接 L2/L3
