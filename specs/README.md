# AES (Agent-Executable Specification) 任务规范

**多 Agent 系统的任务连续性规范** — 让任务在 Agent 间无缝传递、断点恢复、持续演进。

---

## 核心价值

**解决多 Agent 协作的「任务失忆症」**

当多个 Agent 接力完成一个任务时，传统自然语言对话交接存在严重问题：
- Agent 切换时，之前的决策记录丢失
- 长任务链中，信息逐层衰减
- 中断后无法精确从断点继续

AES 通过**结构化的任务状态包**，让任务本身变成可序列化、可交接、可恢复的「接力棒」。

---

## 三种状态

| 状态 | 用途 | 关键字段 |
|------|------|----------|
| **Planning** | 启动新任务/阶段 | intent, success_criteria, implementation_overview |
| **Handoff/Resume** | 中断交接/断点恢复 | state_snapshot, handoff_contract, recovery_policy |
| **Execution Report** | 里程碑完成/审计追踪 | checkpoint_receipts, artifact_registry, validation_rules |

---

## 生态定位

```
A2A (Agent-to-Agent)     → Agent 间通信与委派
MCP (Model Context)       → 工具连接与调用
AES (Agent-Executable)    → 任务背景、约束、交接、进度、报告、断点恢复
```

**简单理解**：
- A2A = 怎么说话
- MCP = 用什么工具  
- AES = 任务是什么、做到哪了

---

## 典型工作流

```
Agent A (规划) ──创建 Planning AES──→ Agent B (实施)
                                            │
                                        (中断/切换)
                                            ↓
Agent C (恢复) ←──加载 Handoff AES───── Agent B
                                            │
                                        (完成)
                                            ↓
Agent D (验收) ──生成 Report AES─────────┘
```

---

## 文档结构

```
AES 文档 = Layer 1 (核心契约) + Layer 2 (实施指令) + Layer 3 (详细计划)

Layer 1:  元数据、状态、意图、参考文献、工件清单、交接契约...
Layer 2:  实施步骤、里程碑检查、文档更新规则...
Layer 3:  问题跟踪、每周计划、实施报告...
```

---

## 核心文件

| 文件 | 说明 |
|------|------|
| [AES_Task_Specification_Writing_Guide.aes.yaml](https://github.com/tetracoralla/agent-system-theory/blob/main/specs/AES_Task_Specification_Writing_Guide.aes.yaml) | Agent 编写 AES 的完整规范 |

---

## 了解更多

- [GitHub 仓库](https://github.com/tetracoralla/agent-system-theory)

---

## 许可证

MIT License
