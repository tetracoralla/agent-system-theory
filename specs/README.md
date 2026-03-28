# AES (Agent-Executable Specification)

**工作流节点执行包** — 让 Agent 高效执行、防止偏离、支持多 Agent 协作。

---

## English Abstract

**AES (Agent-Executable Specification)** is a self-contained workflow node execution package designed for multi-agent systems. Each AES file defines a complete task specification that enables:

- **Efficient Execution**: Agent reads one file and starts working immediately
- **Deviation Prevention**: Explicit constraints and runtime safeguards
- **Seamless Handoff**: Task state preserved across agent switches
- **Enterprise Workflow**: Management agents author, execution agents execute

**Core Innovation**: AES v3.0 transforms task specifications from "planning documents for humans" to "execution packages for agents".

---

## 核心定位

```
AES ≠ 文档
AES = 工作流节点执行包
    = 任务定义 + 约束条件 + 输入输出契约 + 执行提示词 + 风险预防
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

## AES v3.0 结构

### 完整结构

```yaml
# AES v3.0 工作流节点执行包

identity:                    # 节点身份定义
  node_id: "aes-004-ui-design"
  node_name: "UI 设计"
  executor: "designer_agent"
  expected_duration: "2h"

input_contract:              # 输入契约（必须满足才能启动）
  required_artifacts:
    - artifact_id: "prd-v1.0"
      path: "artifacts/prd.md"
      validation: "必须包含页面列表"
  forbidden_assumptions:
    - "不要假设设计风格已确定"

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
```

### 核心字段说明

| 字段 | 用途 | 必需 |
|------|------|------|
| `identity` | 节点身份、执行者、预计时长 | 是 |
| `input_contract` | 启动前必须满足的条件 | 是 |
| `output_contract` | 完成后必须产出的内容 | 是 |
| `execution_constraints` | 防止 Agent 偏离的约束 | 是 |
| `execution_prompt` | 给 Agent 的唯一指令 | 是 |
| `checkpoint` | 质量门禁配置 | 推荐 |
| `safeguards` | 运行时拦截规则 | 推荐 |

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
| [AES_Task_Specification_Writing_Guide.aes.yaml](AES_Task_Specification_Writing_Guide.aes.yaml) | AES v3.0 写作指南（管理层 Agent 必读） |

---

## 版本历史

| 版本 | 变更 |
|------|------|
| v3.0 | 定位为"工作流节点执行包"，新增 input_contract、output_contract、execution_constraints、execution_prompt、safeguards |
| v2.0 | 支持 Planning/Handoff/Execution Report 三种 Profile |
| v1.0 | 基础任务规划文档 |

---

## 许可证

MIT License
