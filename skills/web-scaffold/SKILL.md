---
name: web-scaffold
description: Use when creating, initializing, reviewing, or standardizing TypeScript frontend, React, Vue, Node.js, full-stack, or monorepo projects with shared team engineering conventions.
---

# Web Scaffold

用最少规则建立可维护的 TypeScript 项目。优先级始终是：用户明确选择 > 现有项目约定 > 本 Skill 的新项目默认。

## 边界

- 既有项目只完成当前任务，不迁移构建、样式、包管理、目录或状态方案。
- 新项目只有在用户和框架都未决定时才应用团队默认。
- 不预建空目录、共享层、请求层或状态库；出现真实消费者再提取。
- 缺少关键上下文时，只询问会改变实现方向的问题。

**REQUIRED DESIGN SUB-SKILL:** 当需求、前后端边界、权限、数据模型、部署方式或架构选择仍会改变实现方向时，必须使用 `grill-me`。一次只解决一个决策分支，每问给出推荐答案；能从代码和配置查明的内容直接探索，不询问用户。关键决策收敛并形成方案、获得确认后，才能进入 TDD。范围清楚的局部任务不调用。

**REQUIRED SUB-SKILL:** 实现新功能、Bug 修复、重构或行为变化时，必须使用 `superpowers:test-driven-development`。缺失时停止实现，只安装该子 Skill：

```bash
npx skills add https://github.com/obra/superpowers --skill test-driven-development
```

一次性原型、生成产物、纯文档或纯配置变更只有经用户明确确认才可跳过；功能、Bug、重构和行为变化不属于例外。

## 工作流

1. 识别是新项目还是既有项目，并读取仓库指令、配置和现有约定。
2. 读取 `references/universal.md`。
3. 按技术栈读取最少参考文件：
   - 浏览器端：`references/frontend.md`
   - React：再读 `references/react.md`
   - Vue：再读 `references/vue.md`
   - Node.js：`references/nodejs.md`
4. 存在会改变实现方向的未决问题时，使用 `grill-me` 收敛方案并等待确认。
5. 用 TDD 完成一个可独立验收的目标；超出确认边界时停止并重新确认。
6. 运行项目已有的测试、类型检查、Lint 和构建；没有对应检查时报告缺口，不为当前任务无关地改造工具链。

## 文档与追踪

- 按风险保存长期事实，不批量生成文档模板。
- 业务语义、权限、状态或公共契约变化时，使用 `maintaining-requirement-traceability`。
- 中文文档确需清理 AI 腔时，按 `references/universal.md` 使用 `stop-slop`。

## 可选上游 Skill

复杂框架问题且当前环境已经提供对应 Skill 时，可按需使用：

- React：`vercel-react-best-practices`
- Vue：`vue-best-practices`
- Node.js：`nodejs-backend-patterns`

它们不是项目初始化或普通局部修改的前置条件。

## References

- `references/universal.md`：优先级、TDD、命名、注释、文档和交付门禁。
- `references/frontend.md`：新前端默认、类型、数据访问、结构和验证。
- `references/react.md`：React 差异规则。
- `references/vue.md`：Vue 差异规则。
- `references/nodejs.md`：Node.js 契约、风险和测试规则。
