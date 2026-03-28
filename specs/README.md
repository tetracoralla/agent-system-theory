# AES (Agent-Executable Specification)

**工作流节点执行包** — 让 Agent 高效执行、防止偏离、支持多 Agent 协作。

---

## English Abstract

**AES (Agent-Executable Specification)** is a self-contained workflow node execution package designed for multi-agent systems. Each AES file defines a complete task specification that enables:

- **Efficient Execution**: Agent reads one file and starts working immediately
- **Deviation Prevention**: Explicit constraints and runtime safeguards
- **Seamless Handoff**: Task state preserved across agent switches
- **Enterprise Workflow**: Management agents author, execution agents execute

**Core Innovation**: AES v3.3 transforms task specifications from "planning documents for humans" to "execution packages for agents" with enhanced knowledge management capabilities.

---

## 核心定位

```
AES ≠ 文档
AES = 工作流节点执行包
    = 任务定义 + 约束条件 + 输入输出契约 + 执行提示词 + 风险预防 + 背景知识上下文
```

**服务对象**：

| 对象 | 角色 |
|------|------|
| **Agent（执行者）** | 主要服务对象 — 读取 AES 并执行 |
| 系统（运行时） | 验证、监控、拦截 |
| 人类（需求方） | 查看任务简报（派生件，非 AES 原文） |

---

## 与其他协议的关系

```
┌─────────────────────────────────────────────────────────────┐
│                    协议分层架构                              │
├─────────────────────────────────────────────────────────────┤
│  AES (任务规范层)                                           │
│  ─────────────────                                          │
│  定义"做什么、怎么做、不能做什么"                            │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  A2A (Agent-to-Agent)                                       │
│  ─────────────────                                          │
│  Agent 间通信与委派                                         │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  MCP (Model Context Protocol)                               │
│  ─────────────────                                          │
│  工具连接与调用                                             │
└─────────────────────────────────────────────────────────────┘
```

**简单理解**：
- A2A = 怎么说话
- MCP = 用什么工具
- AES = 任务是什么、怎么做、不能做什么

---

## AES v3.3 结构

### 完整结构

```yaml
# AES v3.3 工作流节点执行包

identity:                    # 节点身份定义
  node_id: "aes-004-ui-design"
  node_name: "UI 设计"
  executor: "designer_agent"
  parent_node: "aes-003-project-planning"
  expected_duration: "2h"

input_contract:              # 输入契约（必须满足才能启动）
  required_artifacts:
    - artifact_id: "prd-v1.0"
      path: "artifacts/prd.md"
      validation: "必须包含页面列表"
  forbidden_assumptions:
    - "不要假设设计风格已确定"
  knowledge_context:         # 背景知识上下文（v3.3新增）
    terminology: [...]       # 术语/缩写解释
    internal_lingo: [...]    # 内部行话/黑话
    background_links: [...]  # 相关背景链接
    similar_tasks: [...]     # 历史类似任务
    domain_nuances: [...]    # 领域特异性知识

output_contract:             # 输出契约（必须产出的内容）
  deliverables:
    - artifact_id: "ui-design-v1.0"
      path: "artifacts/ui/"
  acceptance_criteria:
    - "代码通过设计系统 lint 检查"

execution_constraints:       # 执行约束（防止偏离）
  must_follow:
    - "严格按照设计系统组件库"
  must_not:
    - "禁止引入新的第三方 UI 库"
  max_tokens: 50000
  timeout: "2h"

execution_prompt:            # 执行提示词（唯一指令）
  template: |
    你是一个专业的 UI 设计师 Agent。
    任务：根据 PRD 生成符合设计系统的页面代码。
    ...

checkpoint:                  # 检查点配置
  enabled: true
  type: "design_review"
  approvers: ["product_manager"]

safeguards:                  # 风险预防（运行时拦截）
  deviation_detection:
    - trigger: "npm_install_new_package"
      action: "block"

change_management:            # 变更管理（v3.3新增）
  propagation_mode: "cascade"
  version_compatibility: "backward_compatible"
  rollback_strategy: "revert_to_checkpoint"

# execution_report Profile 必需字段（v3.3新增）
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

# execution_report Profile 可选字段（v3.3新增）
knowledge_capture:
  discoveries: [...]          # 任务中发现的知识
  suggestions: [...]         # 知识库改进建议
```

### 核心字段说明

| 字段 | 用途 | 必需 |
|------|------|------|
| `identity` | 节点身份、执行者、预计时长、上下游关系 | 是 |
| `input_contract` | 启动前必须满足的条件 | 是 |
| `knowledge_context` | 背景知识上下文（v3.3新增，隐性知识显性化） | 推荐 |
| `output_contract` | 完成后必须产出的内容 | 是 |
| `execution_constraints` | 防止 Agent 偏离的约束 | 是 |
| `execution_prompt` | 给 Agent 的唯一指令 | 是 |
| `checkpoint` | 质量门禁配置 | 推荐 |
| `safeguards` | 运行时拦截规则 | 推荐 |
| `change_management` | 变更管理策略（v3.3新增） | 推荐 |
| `execution_summary` | 执行摘要（execution_report Profile 必需） | Profile必需 |
| `execution_metrics` | 执行指标（execution_report Profile 必需） | Profile必需 |
| `output_artifacts` | 输出工件清单（execution_report Profile 必需） | Profile必需 |
| `quality_verification` | 质量验证结果（execution_report Profile 必需） | Profile必需 |
| `knowledge_capture` | 知识捕获（execution_report Profile 可选，v3.3新增） | Profile可选 |

---

## v3.3 新增功能

### 1. knowledge_context（背景知识上下文）

**问题**：企业内部存在大量隐性知识（术语、行话、经验、领域特殊性），新 Agent 或跨部门协作时容易误解。

**解决方案**：在 `input_contract` 中新增 `knowledge_context` 字段，将隐性知识显性化。

```yaml
knowledge_context:
  terminology:                    # 术语/缩写解释
    - term: "SKU"
      definition: "Stock Keeping Unit，库存单位"
      category: "business"
    - term: "PR"
      definition: "Pull Request，代码合并请求"
      category: "technical"

  internal_lingo:                 # 内部行话/黑话
    - phrase: "跑批"
      meaning: "批量数据处理任务"
      context: "通常指夜间执行的ETL任务"
    - phrase: "灰度"
      meaning: "渐进式发布策略"

  background_links:               # 相关背景链接
    - title: "Q4业务规划"
      url: "wiki/business/q4-planning"
      relevance: "理解当前项目优先级"

  similar_tasks:                  # 历史类似任务
    - task_id: "aes-002-similar-project"
      similarity: "相同业务领域"
      lessons: "用户偏好简洁的界面设计"
      reusable_solutions: ["组件库复用"]

  domain_nuances:                 # 领域特异性知识
    - area: "促销活动"
      nuance: "秒杀场景需考虑库存并发扣减"
      implication: "需要分布式锁机制"
```

**价值**：
- 新 Agent 快速理解企业语境
- 消除跨部门协作的术语歧义
- 任务交接时保留背景知识
- 经验复用，避免重复犯错

### 2. change_management（变更管理）

**问题**：工作流节点之间有依赖关系，上游变更后如何传播到下游？

**解决方案**：新增 `change_management` 字段，定义变更传播策略。

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

**四种传播策略**：
- `none`：仅修订当前节点，下游不变（适用于澄清类变更）
- `cascade`：串行下游，等待当前节点完成后修订（适用于架构调整）
- `parallel`：并行下游，标记待修订（适用于业务逻辑变更）
- `stop`：暂停整个工作流分支（适用于重大需求变更）

### 3. knowledge_capture（知识捕获）

**问题**：任务执行中发现的新知识（经验、模式、警告）如何沉淀到知识库？

**解决方案**：在 `execution_report` Profile 中新增 `knowledge_capture` 字段。

```yaml
knowledge_capture:
  discoveries:
    - type: "terminology"
      content: "RPD在内部指'Requirement Planning Document'"
      context: "在需求评审会议中发现"
      confidence: 0.95
      reusable: true

    - type: "pattern"
      content: "用户在移动端更倾向于滑动而非点击"
      context: "分析用户行为数据时发现"
      confidence: 0.85

    - type: "solution"
      content: "使用防抖处理搜索输入，减少API调用"
      context: "解决搜索框性能问题时发现"

    - type: "nuance"
      content: "企业客户的审批流程需要多级嵌套"
      context: "在权限设计时发现"

    - type: "warning"
      content: "第三方库X在特定场景下有内存泄漏"
      context: "性能测试时发现"

  suggestions:
    - area: "terminology"
      suggestion: "增加'跑批'的正式术语定义"
      priority: "medium"
```

**知识类型**：
- `terminology`：新发现的术语/缩写
- `pattern`：可复用的模式/规律
- `solution`：问题解决方案
- `nuance`：领域特异性知识
- `warning`：需要注意的风险/问题

**闭环**：发现 → 捕获 → 审核 → 更新知识库 → 下次任务复用

### 4. 完整的 execution_report Profile

**问题**：任务完成后需要详细的审计追溯和质量验证。

**解决方案**：v3.3 定义了完整的 `execution_report` Profile 必需字段。

```yaml
execution_summary:
  executing_agent: "designer_agent"    # 执行者
  execution_start: "2026-03-28T10:00:00Z"
  execution_end: "2026-03-28T11:30:00Z"
  actual_effort: "1.5小时"
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
    description: "UI 页面代码"
    status: "completed"
    hash: "sha256:def456..."

quality_verification:
  - criteria: "所有 PRD 页面已实现"
    status: "passed"
    evidence: "PRD 列出 3 个页面，实现 3 个页面"
```

---

## 防偏离机制

### 三层防护

```
┌─────────────────────────────────────────────────────────────┐
│                    防偏离三层机制                            │
├─────────────────────────────────────────────────────────────┤
│  Layer 1: AES 自包含                                        │
│  ─────────────────────                                      │
│  • 所有约束在一个文件内                                      │
│  • Agent 只需要读 AES，不需要找其他文件                      │
│                                                             │
│  Layer 2: 最小提示词                                        │
│  ─────────────────────                                      │
│  • execution_prompt 是唯一指令                              │
│  • 明确列出"约束"和"禁止"                                   │
│                                                             │
│  Layer 3: 运行时拦截                                        │
│  ─────────────────────                                      │
│  • safeguards 定义拦截规则                                  │
│  • 系统监控 Agent 行为，发现偏离时拦截                      │
└─────────────────────────────────────────────────────────────┘
```

### 拦截规则示例

```yaml
safeguards:
  deviation_detection:
    - trigger: "npm_install_new_package"
      action: "block"
      message: "禁止引入新的第三方库"
    
    - trigger: "bypass_design_system"
      action: "block"
      message: "必须使用设计系统定义的组件"
    
    - trigger: "skip_checkpoint"
      action: "block"
      message: "必须经过检查点审批"
```

---

## 三种 Profile

| Profile | 阶段 | 用途 |
|---------|------|------|
| **planning** | 任务规划期 | 定义任务、约束、输入输出 |
| **handoff** | 任务交接期 | 状态快照、恢复上下文 |
| **execution_report** | 任务完成期 | 执行结果、审计追溯 |

### Profile 演进

```
planning.aes.yaml → handoff.aes.yaml → execution_report.aes.yaml
     (开始)           (中断/交接)          (完成)
```

---

## 管理层与执行层

### 管理层 Agent

负责撰写 AES，编排工作流，分配任务：

| Agent | 职责 |
|-------|------|
| 产品经理 Agent | 撰写需求级 AES |
| 项目经理 Agent | 撰写规划级 AES，分配给执行层 |
| 技术负责人 Agent | 撰写技术方案 AES |

### 执行层 Agent

负责读取 AES，执行任务，产出 Artifact：

| Agent | 职责 |
|-------|------|
| 设计师 Agent | 执行 UI 设计 AES |
| 前端 Agent | 执行前端开发 AES |
| 后端 Agent | 执行后端开发 AES |
| QA Agent | 执行测试 AES |

---

## 典型工作流

```
用户需求："开发一个电商网站"

管理层 Agent（项目经理）：
├── 分析需求，拆解工作流节点
├── 撰写 AES-1 到 AES-7
└── 分配给执行层 Agent

执行层 Agent 并行执行：
├── AES-4: UI 设计      → 设计师 Agent
├── AES-5: 前端开发     → 前端 Agent
└── AES-6: 后端开发     → 后端 Agent

每个 Subagent 读取分配的 AES 文件并执行
```

---

## 核心设计原则

### 1. 自包含

所有约束、输入输出、禁止行为都在单文件内。Agent 不需要"发挥创造力去找文档"。

### 2. 最小提示词

`execution_prompt` 是给 Agent 的唯一指令，包含所有必要信息。

### 3. 防偏离

通过 `must_follow`、`must_not`、`safeguards` 三层机制防止 Agent 偏离。

### 4. 可拦截

运行时可监控 Agent 行为，发现偏离时主动拦截。

---

## 核心文件

| 文件 | 说明 |
|------|------|
| [AES_Task_Specification_Writing_Guide.aes.yaml](AES_Task_Specification_Writing_Guide.aes.yaml) | AES v3.3 写作指南（管理层 Agent 必读） |

---

## 版本历史

| 版本 | 变更 |
|------|------|
| **v3.3** | 新增 `knowledge_context`（背景知识上下文）、`change_management`（变更管理）、`knowledge_capture`（知识捕获），完善 `execution_report` Profile 必需字段 |
| v3.0 | 定位为"工作流节点执行包"，新增 input_contract、output_contract、execution_constraints、execution_prompt、safeguards |
| v2.0 | 支持 Planning/Handoff/Execution Report 三种 Profile |
| v1.0 | 基础任务规划文档 |

---

## 作者

由 [openAdam](https://github.com/tetracoralla) 与 AI 智能体协作编写。

---

## 许可证

MIT License
