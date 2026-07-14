---
name: maintaining-requirement-traceability
description: Use when requirements and implementations drift, multiple artifacts or runtimes must preserve the same behavior, an implementation is being migrated, or a team needs lightweight traceability without a heavyweight spec workflow.
---

# 维护需求可追溯性

维护一条最小、稳定、可验证的关系链：需求来源 → 需求标识 → 契约或实现锚点 → 当前实现 → 验证证据。只保存无法可靠推导的关系。

## 通用模型

| 层级 | 回答的问题 | 常见形式 |
|---|---|---|
| 来源 | 当前规则由谁定义 | 文档、Issue、工单、决策记录 |
| 标识 | 如何稳定引用这条需求 | 需求 ID、Issue ID、能力 ID |
| 锚点 | 从哪里进入系统边界或实现 | API、事件、Schema、命令、路由、任务、符号 |
| 实现 | 当前哪些代码落实规则 | 通过项目已有工具动态发现 |
| 证据 | 什么证明实现符合需求 | 行为测试、契约测试、验收记录 |

关系不必一一对应：一条需求可以有多个锚点和证据；多个需求也可以共享一个能力。不要为追求形式上的一一对应扭曲真实关系。

## 工作流

### 1. 判断是否需要长期追踪

追踪用户可观察行为、业务规则、权限、安全、合规、状态流转、公共契约，以及跨系统或跨实现必须保持的语义。普通重构、格式化、生成产物刷新和不改变行为的技术调整通常不新增关系。

### 2. 确定当前有效来源

复用项目已有的文档、Issue 或登记系统。只有现有来源不可更新或新需求明确取代旧需求时才新增来源，并记录替代关系。代码现状和线上行为是证据，不自动等于正确需求。

### 3. 选择最小稳定锚点

按实际架构选择一个或多个锚点：

- 契约：API operation、GraphQL operation、事件类型、消息 Schema、数据约束、CLI 命令。
- 运行入口：路由、任务名、处理器、导出符号。
- 验证入口：测试 ID、验收场景 ID。

锚点必须稳定且可检索。不要虚构项目不存在的接口，也不要记录完整源码文件清单。

### 4. 动态发现当前实现

优先使用项目已有的结构化代码图、LSP 引用、静态分析或构建依赖图；不可用时使用符号和文本检索，再阅读命中的核心实现。记录稳定入口，不复制展开后的调用链和文件路径。

### 5. 连接验收规则与证据

区分“应该满足什么”和“由什么证明”：`acceptance` 保存可观察规则，`evidence` 指向测试或验收记录。类型一致、代码存在或测试文件存在都不等于需求已经满足；必须检查相应证据的结果。

### 6. 报告冲突

发现来源、契约、实现和证据不一致时，列出冲突、影响和缺失信息。没有明确裁决权时标记待确认，不根据当前代码反向编造需求。

## 可选索引

只有跨文档或跨系统检索成为真实需要时，才在项目约定的位置维护索引；已有等价机制则复用。下面只是关系示例，不是强制文件名或 Schema：

```yaml
requirements:
  - id: REQ-017
    title: 尚未完成的操作可以撤销
    sources: [issue:417]
    anchors:
      - type: api-operation
        value: cancelOperation
      - type: event
        value: operation.cancelled
    acceptance:
      - 已完成的操作不能撤销
      - 重复撤销不会产生重复副作用
    evidence:
      - type: behavior-test
        value: REQ-017-cancel-operation
```

字段按实际需要裁剪，不复制需求正文、实现文件列表或测试结果快照。

## 工具适配

- OpenAPI 项目可使用 `operationId`；GraphQL、AsyncAPI、Schema Registry 或 CLI 项目使用各自稳定标识。
- 客户端或模型生成器可从契约派生产物，不再人工维护映射。
- CodeGraph 等代码图只是动态发现方式之一；没有代码图时使用 LSP、静态分析、构建工具或文本检索。

这些工具都不是使用本 Skill 的前置条件。

## 第三方知识库

默认沿用现有版本化事实来源。只有访问、跨仓库检索、权限隔离、审批或外部协作已成为真实瓶颈时才引入知识库。必须指定唯一可编辑来源；其他系统优先做可重建的单向投影，禁止双向编辑和复制源码关系。

## 边界

- 不强制使用 OpenSpec 或固定的 `proposal / design / tasks / archive` 流程。
- 不要求每次改动创建 Spec、设计文档、索引或任务文档。
- 不替代项目的方案确认、测试、代码审查和变更管理要求。
