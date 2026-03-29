# AES (Agent-Executable Specification)

**工作流节点执行包** — 让 Agent 高效执行、防止偏离、支持多 Agent 协作。

**Workflow Node Execution Package** — Enabling agents to execute efficiently, prevent deviation, and support multi-agent collaboration.

---

## English Abstract | 英文摘要

**AES (Agent-Executable Specification)** is a self-contained workflow node execution package designed for multi-agent systems. Each AES file defines a complete task specification that enables:

- **Efficient Execution**: Agent reads one file and starts working immediately
- **Deviation Prevention**: Explicit constraints and runtime safeguards
- **Seamless Handoff**: Task state preserved across agent switches
- **Enterprise Workflow**: Management agents author, execution agents execute

**Core Innovation**: AES v3.4 transforms task specifications from "planning documents for humans" to "execution packages for agents" with enhanced knowledge management capabilities.

**AES（Agent-Executable Specification）** 是一个自包含的工作流节点执行包，专为多智能体系统设计。每个 AES 文件定义了一个完整的任务规范，支持：

- **高效执行**：Agent 读取单个文件即可开始工作
- **防止偏离**：明确的约束和运行时安全防护
- **无缝交接**：任务状态在 Agent 切换间保持
- **企业工作流**：管理层 Agent 编写，执行层 Agent 执行

**核心创新**：AES v3.4 将任务规范从"面向人类的规划文档"转变为"面向 Agent 的执行包"，具备增强的知识管理能力。

---

## Core Positioning | 核心定位

```
AES ≠ 文档 (Document)
AES = 工作流节点执行包 (Workflow Node Execution Package)
    = 任务定义 + 约束条件 + 输入输出契约 + 执行提示词 + 风险预防 + 背景知识上下文
    = Task Definition + Constraints + Input/Output Contracts + Execution Prompts + Risk Prevention + Knowledge Context
```

**服务对象 | Target Audience**：

| 对象 | 角色 | Object | Role |
|------|------|--------|------|
| **Agent（执行者）** | 主要服务对象 — 读取 AES 并执行 | **Agent (Executor)** | Primary target — reads AES and executes |
| 系统（运行时） | 验证、监控、拦截 | System (Runtime) | Validates, monitors, intercepts |
| 人类（需求方） | 查看任务简报（派生件，非 AES 原文） | Human (Requester) | Views task briefings (derived artifacts, not AES source) |

---

## Relationship with Other Protocols | 与其他协议的关系

```
┌─────────────────────────────────────────────────────────────┐
│                    协议分层架构                              │
│                    Protocol Layered Architecture            │
├─────────────────────────────────────────────────────────────┤
│  AES (任务规范层)                                           │
│  AES (Task Specification Layer)                            │
│  ─────────────────                                          │
│  定义"做什么、怎么做、不能做什么"                            │
│  Defines "what to do, how to do it, what not to do"         │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  A2A (Agent-to-Agent)                                       │
│  A2A (Agent-to-Agent)                                       │
│  ─────────────────                                          │
│  Agent 间通信与委派                                         │
│  Inter-agent communication and delegation                   │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  MCP (Model Context Protocol)                               │
│  MCP (Model Context Protocol)                               │
│  ─────────────────                                          │
│  工具连接与调用                                             │
│  Tool connection and invocation                             │
└─────────────────────────────────────────────────────────────┘
```

**简单理解 | Simple Understanding**：
- A2A = 怎么说话 (How to talk)
- MCP = 用什么工具 (What tools to use)
- AES = 任务是什么、怎么做、不能做什么 (What the task is, how to do it, what not to do)

---

## AES v3.4 Structure | AES v3.4 结构

### Complete Structure | 完整结构

```yaml
# AES v3.4 工作流节点执行包 | AES v3.4 Workflow Node Execution Package

identity:                    # 节点身份定义 | Node Identity Definition
  node_id: "aes-004-ui-design"
  node_name: "UI 设计 | UI Design"
  executor: "designer_agent"
  parent_node: "aes-003-project-planning"
  expected_duration: "2h"

input_contract:              # 输入契约（必须满足才能启动） | Input Contract (must be satisfied to start)
  required_artifacts:
    - artifact_id: "prd-v1.0"
      path: "artifacts/prd.md"
      validation: "必须包含页面列表 | Must contain page list"
  forbidden_assumptions:
    - "不要假设设计风格已确定 | Do not assume design style is determined"
  knowledge_context:         # 背景知识上下文（v3.4增强） | Knowledge Context (v3.4 enhanced)
    terminology: [...]       # 术语/缩写解释 | Terminology/Abbreviation Explanations
    internal_lingo: [...]    # 内部行话/黑话 | Internal Lingo/Jargon
    background_links: [...]  # 相关背景链接 | Relevant Background Links
    similar_tasks: [...]     # 历史类似任务 | Historical Similar Tasks
    domain_nuances: [...]    # 领域特异性知识 | Domain-Specific Knowledge

output_contract:             # 输出契约（必须产出的内容） | Output Contract (must deliver content)
  deliverables:
    - artifact_id: "ui-design-v1.0"
      path: "artifacts/ui/"
  acceptance_criteria:
    - "代码通过设计系统 lint 检查 | Code passes design system lint check"

execution_constraints:       # 执行约束（防止偏离） | Execution Constraints (prevent deviation)
  must_follow:
    - "严格按照设计系统组件库 | Strictly follow design system component library"
  must_not:
    - "禁止引入新的第三方 UI 库 | Prohibit introducing new third-party UI libraries"
  max_tokens: 50000
  timeout: "2h"

execution_prompt:            # 执行提示词（唯一指令） | Execution Prompt (single instruction)
  template: |
    你是一个专业的 UI 设计师 Agent。 | You are a professional UI designer Agent.
    任务：根据 PRD 生成符合设计系统的页面代码。 | Task: Generate page code according to the design system based on PRD.
    ...

checkpoint:                  # 检查点配置 | Checkpoint Configuration
  enabled: true
  type: "design_review"
  approvers: ["product_manager"]

safeguards:                  # 风险预防（运行时拦截） | Risk Prevention (runtime interception)
  deviation_detection:
    - trigger: "npm_install_new_package"
      action: "block"

change_management:            # 变更管理（v3.4优化） | Change Management (v3.4 optimized)
  propagation_mode: "cascade"
  version_compatibility: "backward_compatible"
  rollback_strategy: "revert_to_checkpoint"

# execution_report Profile 必需字段（v3.4完善） | execution_report Profile Required Fields (v3.4 refined)
execution_summary:
  executing_agent: string
  execution_start: timestamp
  execution_end: timestamp
  actual_effort: string
  tasks_completed: list
  tasks_skipped: list

execution_metrics:
  duration: string
  tokens_used: integer
  files_created: integer
  files_modified: integer
  test_coverage: integer

output_artifacts:
  - path: string
    description: string
    status: string
    hash: string

quality_verification:
  - criteria: string
    status: string
    evidence: string

# execution_report Profile 可选字段（v3.4增强） | execution_report Profile Optional Fields (v3.4 enhanced)
knowledge_capture:
  discoveries: [...]          # 任务中发现的知识 | Knowledge discovered during task
  suggestions: [...]         # 知识库改进建议 | Knowledge base improvement suggestions
```

### Core Field Description | 核心字段说明

| 字段 | 用途 | 必需 | Field | Purpose | Required |
|------|------|------|-------|---------|----------|
| `identity` | 节点身份、执行者、预计时长、上下游关系 | 是 | Node identity, executor, expected duration, upstream/downstream relationships | Yes |
| `input_contract` | 启动前必须满足的条件 | 是 | Conditions that must be satisfied before starting | Yes |
| `knowledge_context` | 背景知识上下文（隐性知识显性化） | 推荐 | Knowledge context (making implicit knowledge explicit) | Recommended |
| `output_contract` | 完成后必须产出的内容 | 是 | Content that must be delivered upon completion | Yes |
| `execution_constraints` | 防止 Agent 偏离的约束 | 是 | Constraints to prevent agent deviation | Yes |
| `execution_prompt` | 给 Agent 的唯一指令 | 是 | Single instruction for the agent | Yes |
| `checkpoint` | 质量门禁配置 | 推荐 | Quality gate configuration | Recommended |
| `safeguards` | 运行时拦截规则 | 推荐 | Runtime interception rules | Recommended |
| `change_management` | 变更管理策略 | 推荐 | Change management strategy | Recommended |
| `execution_summary` | 执行摘要（execution_report Profile 必需） | Profile必需 | Execution summary (execution_report Profile required) | Profile Required |
| `execution_metrics` | 执行指标（execution_report Profile 必需） | Profile必需 | Execution metrics (execution_report Profile required) | Profile Required |
| `output_artifacts` | 输出工件清单（execution_report Profile 必需） | Profile必需 | Output artifacts list (execution_report Profile required) | Profile Required |
| `quality_verification` | 质量验证结果（execution_report Profile 必需） | Profile必需 | Quality verification results (execution_report Profile required) | Profile Required |
| `knowledge_capture` | 知识捕获（execution_report Profile 可选） | Profile可选 | Knowledge capture (execution_report Profile optional) | Profile Optional |

---

## v3.4 New Features | v3.4 新增功能

### 1. knowledge_context（背景知识上下文）

**问题 | Problem**：企业内部存在大量隐性知识（术语、行话、经验、领域特殊性），新 Agent 或跨部门协作时容易误解。
There is a large amount of implicit knowledge within enterprises (terminology, jargon, experience, domain-specific knowledge), which new agents or cross-departmental collaboration can easily misunderstand.

**解决方案 | Solution**：在 `input_contract` 中新增 `knowledge_context` 字段，将隐性知识显性化。
Added the `knowledge_context` field in `input_contract` to make implicit knowledge explicit.

```yaml
knowledge_context:
  terminology:                    # 术语/缩写解释 | Terminology/Abbreviation Explanations
    - term: "SKU"
      definition: "Stock Keeping Unit，库存单位 | Stock Keeping Unit, inventory unit"
      category: "business"
    - term: "PR"
      definition: "Pull Request，代码合并请求 | Pull Request, code merge request"
      category: "technical"

  internal_lingo:                 # 内部行话/黑话 | Internal Lingo/Jargon
    - phrase: "跑批"
      meaning: "批量数据处理任务 | Batch data processing task"
      context: "通常指夜间执行的ETL任务 | Usually refers to ETL tasks executed at night"
    - phrase: "灰度"
      meaning: "渐进式发布策略 | Progressive release strategy"

  background_links:               # 相关背景链接 | Relevant Background Links
    - title: "Q4业务规划 | Q4 Business Planning"
      url: "wiki/business/q4-planning"
      relevance: "理解当前项目优先级 | Understand current project priorities"

  similar_tasks:                  # 历史类似任务 | Historical Similar Tasks
    - task_id: "aes-002-similar-project"
      similarity: "相同业务领域 | Same business domain"
      lessons: "用户偏好简洁的界面设计 | Users prefer clean interface design"
      reusable_solutions: ["组件库复用 | Component library reuse"]

  domain_nuances:                 # 领域特异性知识 | Domain-Specific Knowledge
    - area: "促销活动 | Promotional Activities"
      nuance: "秒杀场景需考虑库存并发扣减 | Flash sale scenarios require consideration of inventory concurrent deduction"
      implication: "需要分布式锁机制 | Requires distributed lock mechanism"
```

**价值 | Value**：
- 新 Agent 快速理解企业语境 | New agents quickly understand enterprise context
- 消除跨部门协作的术语歧义 | Eliminate terminology ambiguity in cross-departmental collaboration
- 任务交接时保留背景知识 | Preserve background knowledge during task handoff
- 经验复用，避免重复犯错 | Experience reuse, avoid repeating mistakes

### 2. change_management（变更管理）

**问题 | Problem**：工作流节点之间有依赖关系，上游变更后如何传播到下游？
Workflow nodes have dependencies; how are upstream changes propagated to downstream?

**解决方案 | Solution**：新增 `change_management` 字段，定义变更传播策略。
Added `change_management` field to define change propagation strategies.

```yaml
change_management:
  propagation_mode: "cascade"  # none/cascade/parallel/stop
  version_compatibility: "backward_compatible"
  rollback_strategy: "revert_to_checkpoint"
  notification_targets:
    - "executor"
    - "child_nodes"
    - "workflow_orchestrator"
```

**四种传播策略 | Four Propagation Strategies**：
- `none`：仅修订当前节点，下游不变（适用于澄清类变更） | Revise only current node, downstream unchanged (suitable for clarification changes)
- `cascade`：串行下游，等待当前节点完成后修订（适用于架构调整） | Serial downstream, wait for current node completion before revision (suitable for architectural adjustments)
- `parallel`：并行下游，标记待修订（适用于业务逻辑变更） | Parallel downstream, mark for revision (suitable for business logic changes)
- `stop`：暂停整个工作流分支（适用于重大需求变更） | Pause entire workflow branch (suitable for major requirement changes)

### 3. knowledge_capture（知识捕获）

**问题 | Problem**：任务执行中发现的新知识（经验、模式、警告）如何沉淀到知识库？
How is new knowledge discovered during task execution (experience, patterns, warnings) captured into the knowledge base?

**解决方案 | Solution**：在 `execution_report` Profile 中新增 `knowledge_capture` 字段。
Added `knowledge_capture` field in the `execution_report` Profile.

```yaml
knowledge_capture:
  discoveries:
    - type: "terminology"
      content: "RPD在内部指'Requirement Planning Document' | RPD internally refers to 'Requirement Planning Document'"
      context: "在需求评审会议中发现 | Discovered during requirements review meeting"
      confidence: 0.95
      reusable: true

    - type: "pattern"
      content: "用户在移动端更倾向于滑动而非点击 | Users prefer swiping over clicking on mobile"
      context: "分析用户行为数据时发现 | Discovered when analyzing user behavior data"
      confidence: 0.85

    - type: "solution"
      content: "使用防抖处理搜索输入，减少API调用 | Use debouncing for search input to reduce API calls"
      context: "解决搜索框性能问题时发现 | Discovered when solving search box performance issues"
      confidence: 0.90

    - type: "nuance"
      content: "企业客户的审批流程需要多级嵌套 | Enterprise customer approval processes require multi-level nesting"
      context: "在权限设计时发现 | Discovered during permission design"
      confidence: 0.80

    - type: "warning"
      content: "第三方库X在特定场景下有内存泄漏 | Third-party library X has memory leaks in specific scenarios"
      context: "性能测试时发现 | Discovered during performance testing"
      confidence: 0.95

  suggestions:
    - area: "terminology"
      suggestion: "增加'跑批'的正式术语定义 | Add formal terminology definition for 'batch processing'"
      priority: "medium"
```

**知识类型 | Knowledge Types**：
- `terminology`：新发现的术语/缩写 | Newly discovered terminology/abbreviations
- `pattern`：可复用的模式/规律 | Reusable patterns/regularities
- `solution`：问题解决方案 | Problem solutions
- `nuance`：领域特异性知识 | Domain-specific knowledge
- `warning`：需要注意的风险/问题 | Risks/issues to be aware of

**闭环 | Closed Loop**：发现 → 捕获 → 审核 → 更新知识库 → 下次任务复用 | Discover → Capture → Review → Update Knowledge Base → Reuse in Next Task

### 4. 完整的 execution_report Profile | Complete execution_report Profile

**问题 | Problem**：任务完成后需要详细的审计追溯和质量验证。
Detailed audit trail and quality verification are needed after task completion.

**解决方案 | Solution**：v3.4 定义了完整的 `execution_report` Profile 必需字段。
v3.4 defines complete `execution_report` Profile required fields.

```yaml
execution_summary:
  executing_agent: "designer_agent"    # 执行者 | Executor
  execution_start: "2026-03-28T10:00:00Z"
  execution_end: "2026-03-28T11:30:00Z"
  actual_effort: "1.5小时 | 1.5 hours"
  tasks_completed: [...]
  tasks_skipped: [...]

execution_metrics:
  duration: "1.5h"
  tokens_used: 35000
  files_created: 12
  files_modified: 3
  test_coverage: 85

output_artifacts:
  - path: "artifacts/ui/"
    description: "UI 页面代码 | UI page code"
    status: "completed"
    hash: "sha256:def456..."

quality_verification:
  - criteria: "所有 PRD 页面已实现 | All PRD pages implemented"
    status: "passed"
    evidence: "PRD 列出 3 个页面，实现 3 个页面 | PRD lists 3 pages, 3 pages implemented"
```

---

## Deviation Prevention Mechanism | 防偏离机制

### Three-Layer Protection | 三层防护

```
┌─────────────────────────────────────────────────────────────┐
│                    防偏离三层机制                            │
│                    Three-Layer Deviation Prevention         │
├─────────────────────────────────────────────────────────────┤
│  Layer 1: AES 自包含                                        │
│  Layer 1: AES Self-Contained                                │
│  ─────────────────────                                      │
│  • 所有约束在一个文件内                                      │
│  • All constraints in one file                              │
│  • Agent 只需要读 AES，不需要找其他文件                      │
│  • Agent only needs to read AES, no need to find other files│
│                                                             │
│  Layer 2: 最小提示词                                        │
│  Layer 2: Minimal Prompt                                    │
│  ─────────────────────                                      │
│  • execution_prompt 是唯一指令                              │
│  • execution_prompt is the only instruction                │
│  • 明确列出"约束"和"禁止"                                   │
│  • Explicitly lists "must follow" and "must not"            │
│                                                             │
│  Layer 3: 运行时拦截                                        │
│  Layer 3: Runtime Interception                              │
│  ─────────────────────                                      │
│  • safeguards 定义拦截规则                                  │
│  • safeguards define interception rules                     │
│  • 系统监控 Agent 行为，发现偏离时拦截                      │
│  • System monitors agent behavior, intercepts when deviation│
└─────────────────────────────────────────────────────────────┘
```

### Interception Rule Examples | 拦截规则示例

```yaml
safeguards:
  deviation_detection:
    - trigger: "npm_install_new_package"
      action: "block"
      message: "禁止引入新的第三方库 | Prohibit introducing new third-party libraries"
    
    - trigger: "bypass_design_system"
      action: "block"
      message: "必须使用设计系统定义的组件 | Must use design system defined components"
    
    - trigger: "skip_checkpoint"
      action: "block"
      message: "必须经过检查点审批 | Must pass checkpoint approval"
```

---

## Three Profiles | 三种 Profile

| Profile | 阶段 | 用途 | Phase | Purpose |
|---------|------|------|-------|---------|
| **planning** | 任务规划期 | 定义任务、约束、输入输出 | Task Planning Phase | Define tasks, constraints, inputs/outputs |
| **handoff** | 任务交接期 | 状态快照、恢复上下文 | Task Handoff Phase | State snapshot, context recovery |
| **execution_report** | 任务完成期 | 执行结果、审计追溯 | Task Completion Phase | Execution results, audit trail |

### Profile Evolution | Profile 演进

```
planning.aes.yaml → handoff.aes.yaml → execution_report.aes.yaml
     (开始)           (中断/交接)          (完成)
     (Start)         (Interruption/Handoff) (Completion)
```

---

## Management vs Execution Layers | 管理层与执行层

### Management Layer Agents | 管理层 Agent

负责撰写 AES，编排工作流，分配任务：
Responsible for writing AES, orchestrating workflows, assigning tasks:

| Agent | 职责 | Responsibility |
|-------|------|----------------|
| 产品经理 Agent | 撰写需求级 AES | Write requirement-level AES |
| Product Manager Agent | | |
| 项目经理 Agent | 撰写规划级 AES，分配给执行层 | Write planning-level AES, assign to execution layer |
| Project Manager Agent | | |
| 技术负责人 Agent | 撰写技术方案 AES | Write technical solution AES |
| Technical Lead Agent | | |

### Execution Layer Agents | 执行层 Agent

负责读取 AES，执行任务，产出 Artifact：
Responsible for reading AES, executing tasks, producing artifacts:

| Agent | 职责 | Responsibility |
|-------|------|----------------|
| 设计师 Agent | 执行 UI 设计 AES | Execute UI design AES |
| Designer Agent | | |
| 前端 Agent | 执行前端开发 AES | Execute frontend development AES |
| Frontend Agent | | |
| 后端 Agent | 执行后端开发 AES | Execute backend development AES |
| Backend Agent | | |
| QA Agent | 执行测试 AES | Execute testing AES |
| QA Agent | | |

---

## Typical Workflow | 典型工作流

```
用户需求："开发一个电商网站" | User Requirement: "Develop an e-commerce website"

管理层 Agent（项目经理）： | Management Layer Agent (Project Manager):
├── 分析需求，拆解工作流节点 | Analyze requirements, decompose workflow nodes
├── 撰写 AES-1 到 AES-7 | Write AES-1 to AES-7
└── 分配给执行层 Agent | Assign to execution layer agents

执行层 Agent 并行执行： | Execution Layer Agents Execute in Parallel:
├── AES-4: UI 设计 → 设计师 Agent | AES-4: UI Design → Designer Agent
├── AES-5: 前端开发 → 前端 Agent | AES-5: Frontend Development → Frontend Agent
└── AES-6: 后端开发 → 后端 Agent | AES-6: Backend Development → Backend Agent

每个 Subagent 读取分配的 AES 文件并执行 | Each subagent reads assigned AES file and executes
```

---

## Core Design Principles | 核心设计原则

### 1. 自包含 | Self-Contained
所有约束、输入输出、禁止行为都在单文件内。Agent 不需要"发挥创造力去找文档"。
All constraints, inputs/outputs, prohibited behaviors are in a single file. Agent doesn't need to "get creative to find documentation".

### 2. 最小提示词 | Minimal Prompt
`execution_prompt` 是给 Agent 的唯一指令，包含所有必要信息。
`execution_prompt` is the single instruction for the agent, containing all necessary information.

### 3. 防偏离 | Deviation Prevention
通过 `must_follow`、`must_not`、`safeguards` 三层机制防止 Agent 偏离。
Prevents agent deviation through three-layer mechanism: `must_follow`, `must_not`, `safeguards`.

### 4. 可拦截 | Interceptable
运行时可监控 Agent 行为，发现偏离时主动拦截。
Runtime monitoring of agent behavior, actively intercepting when deviation is detected.

---

## Core Files | 核心文件

| 文件 | 说明 | File | Description |
|------|------|------|-------------|
| [AES_Task_Specification_Writing_Guide.aes.yaml](AES_Task_Specification_Writing_Guide.aes.yaml) | AES v3.4 写作指南（管理层 Agent 必读） | AES v3.4 Writing Guide (Management Layer Agents Required Reading) |

---

## Version History | 版本历史

| 版本 | 变更 | Version | Changes |
|------|------|---------|---------|
| **v3.4** | 知识上下文增强、变更管理优化、知识捕获完善、执行报告完善 | **v3.4** | Enhanced knowledge context, optimized change management, refined knowledge capture, improved execution report |
| **v3.3** | 新增 `knowledge_context`（背景知识上下文）、`change_management`（变更管理）、`knowledge_capture`（知识捕获），完善 `execution_report` Profile 必需字段 | **v3.3** | Added `knowledge_context`, `change_management`, `knowledge_capture`, refined `execution_report` Profile required fields |
| v3.0 | 定位为"工作流节点执行包"，新增 input_contract、output_contract、execution_constraints、execution_prompt、safeguards | v3.0 | Positioned as "workflow node execution package", added input_contract, output_contract, execution_constraints, execution_prompt, safeguards |
| v2.0 | 支持 Planning/Handoff/Execution Report 三种 Profile | v2.0 | Supports three Profiles: Planning/Handoff/Execution Report |
| v1.0 | 基础任务规划文档 | v1.0 | Basic task planning document |

---

## Author | 作者

由 [openAdam](https://github.com/tetracoralla) 与 AI 智能体协作编写。
Written by [openAdam](https://github.com/tetracoralla) in collaboration with AI agents.

---

## License | 许可证

Creative Commons Attribution 4.0 International (CC BY 4.0)

This work is licensed under the Creative Commons Attribution 4.0 International License.
To view a copy of this license, visit: https://creativecommons.org/licenses/by/4.0/

**关键许可条款 | Key License Terms**:
- **共享 | Share** — 在任何媒介或格式中复制和重新分发材料 | Copy and redistribute the material in any medium or format
- **改编 | Adapt** — 对材料进行混合、转换和再创作，可用于任何目的 | Remix, transform, and build upon the material for any purpose
- **署名 | Attribution** — 您必须给出适当的署名，提供指向本许可的链接，并说明是否进行了更改 | You must give appropriate credit, provide a link to the license, and indicate if changes were made

**具体署名要求 | Specific Attribution Requirement**:
作者：openAdam | Author: openAdam  
来源：https://github.com/tetracoralla/agent-system-theory | Source: https://github.com/tetracoralla/agent-system-theory

