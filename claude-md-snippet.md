## Skill Fusion: gstack + superpowers

两套框架各取所长，硬路由，不靠模型自觉。

### 核心原则

gstack 包裹两端（决策 + 审查），superpowers 负责中间（执行纪律）。重叠能力只走指定的一方，另一方的对应 skill 禁止触发。

### 路由表（硬规则）

| 阶段 | 用什么 | 具体 skill | 禁止触发 |
|---|---|---|---|
| 头脑风暴 | gstack | `/office-hours` | `superpowers:brainstorming` |
| 计划策略 | gstack | `/plan-ceo-review` | `superpowers:writing-plans`（计划策略场景） |
| 计划架构 | gstack | `/plan-eng-review` | `superpowers:writing-plans`（计划架构场景） |
| 计划设计 | gstack | `/plan-design-review` | `superpowers:writing-plans`（计划设计场景） |
| 调试 | gstack | `/investigate` | `superpowers:systematic-debugging` |
| 代码审查 | gstack | `/review` | `superpowers:requesting-code-review` |
| 发布/PR | gstack | `/ship` | `superpowers:finishing-a-development-branch` |
| 计划执行 | superpowers | `executing-plans` | — |
| TDD | superpowers | `test-driven-development` | — |
| 并行分发 | superpowers | `dispatching-parallel-agents` | — |
| 完成前验证 | superpowers | `verification-before-completion` | — |
| Git worktree | superpowers | `using-git-worktrees` | — |
| 子代理协调 | superpowers | `subagent-driven-development` | — |
| 处理审查反馈 | superpowers | `receiving-code-review` | — |

### 完整工作流管线

```
想法 → gstack /office-hours         (头脑风暴)
     → gstack /plan-ceo-review      (策略审查)
     → gstack /plan-eng-review      (架构审查)
     → gstack /plan-design-review   (设计审查)
     → superpowers executing-plans   (带检查点执行)
       ├─ superpowers TDD           (测试驱动实现)
       ├─ superpowers parallel-agents(并行子任务)
       └─ superpowers verification   (完成前验证)
     → gstack /review               (代码审查)
     → gstack /ship                 (发布)
     → gstack /qa                   (上线后 QA)
     → gstack /canary               (金丝雀监控)
```

不是每个任务都要走完整管线。触发规则：
- 小任务（<3 步）：直接执行，跳过头脑风暴和计划审查
- 中任务（3-7 步）：从 `/plan-eng-review` 开始，走执行+审查+发布
- 大任务（>7 步）：走完整管线

### gstack 独占能力（无 superpowers 替代）

- `/browse` — 无头浏览器（所有 web 浏览用这个，禁止 `mcp__claude-in-chrome__*`）
- `/qa`, `/qa-only` — 浏览器 QA 测试
- `/design-review`, `/design-consultation` — 视觉设计
- `/cso` — 安全审计
- `/benchmark`, `/canary` — 性能/监控
- `/retro` — 周回顾
- `/land-and-deploy` — 部署流程
- `/careful`, `/freeze`, `/guard`, `/unfreeze` — 安全护栏
- `/autoplan` — 自动审查流水线
- `/document-release` — 发布后文档更新
- `/codex` — 第二意见 / 对抗性代码审查
- `/gstack-upgrade` — 升级 gstack

### superpowers 独占能力（无 gstack 替代）

- `superpowers-lab:*` — tmux 交互、重复函数检测、MCP CLI、Slack
- `superpowers-developing-for-claude-code:*` — Claude Code 开发相关
- `skill-creator:skill-creator` — 技能创建
