---
name: team-project-scaffold
description: "团队新项目脚手架和工程规范 Skill。Use when creating, initializing, reviewing, or standardizing frontend, React, Vue, Node.js, full-stack, documentation-driven, TypeScript, Tailwind, Rsbuild, Biome, Swagger, Zod, TanStack Query, or China-oriented enterprise projects."
---

# Team Project Scaffold

用这个 skill 把团队的新项目初始化、技术选型、目录命名、文档驱动开发、前后端工程规范落到可执行任务里。

## When to Use This Skill

在这些场景触发：
- 用户要创建、初始化、接入、重构或审查一个团队新项目。
- 用户提到脚手架、项目模板、工程规范、目录规范、技术选型、文档驱动开发。
- 项目涉及前端、React、Vue、Node.js 后端、TypeScript、Tailwind CSS、Rsbuild、Biome、Swagger、Zod、TanStack Query。
- 用户需要把项目接入中国国内企业研发环境，例如私有知识库、国内 npm 源、前后端联调、部署文档。

## Not For / Boundaries

- 不替代已经成熟且被明确指定的第三方 skill。遇到 React、Vue、Node.js、中文文档润色等场景，先按参考文件安装指定 skill，再补充本团队的差异化约束。
- 不在没有需求上下文时擅自生成完整业务系统。缺少关键信息时，先问 1-3 个问题：项目类型、目标框架、已有组件库或后端框架。
- 不绕开用户指定的技术栈。如果用户明确要求 Vite、ESLint、Prettier、npm 等方案，先说明本 skill 的默认偏好，再按用户决策执行。
- 不写含糊的“最佳实践”堆砌。每条规范都要落到文件、配置、命令、代码结构或验收标准。

## Quick Reference

先执行通用工作流：
1. 确认项目类型：frontend、react、vue、nodejs、full-stack 或 monorepo。
2. 读取 `references/universal.md`，应用所有项目通用约束。
3. 如果有前端，读取 `references/frontend.md`。
4. 如果是 React，读取 `references/react.md`，并先安装指定 React skill。
5. 如果是 Vue，读取 `references/vue.md`，并先安装指定 Vue skill。
6. 如果是 Node.js 后端，读取 `references/nodejs.md`，并先安装指定 Node.js skill。
7. 建立或补齐文档目录，再开始代码变更。
8. 交付前运行项目已有质量检查；没有检查脚本时，至少说明缺口并补齐建议脚本。

安装本 skill 的仓库结构必须保持：

```text
skills/
  team-project-scaffold/
    SKILL.md
    references/
      index.md
      universal.md
      frontend.md
      react.md
      vue.md
      nodejs.md
```

安装命令格式：

```bash
npx skills add https://github.com/<org>/<repo> --skill team-project-scaffold
```

必须优先安装的基础 skill：

```bash
npx skills add https://github.com/obra/superpowers
```

文档编写或润色时安装：

```bash
npx skills add https://github.com/op7418/humanizer-zh --skill humanizer-zh
```

React 项目安装：

```bash
npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices
```

Vue 项目安装：

```bash
npx skills add https://github.com/hyf0/vue-skills --skill vue-best-practices
```

Node.js 后端项目安装：

```bash
npx skills add https://github.com/wshobson/agents --skill nodejs-backend-patterns
```

## Examples

### Example 1: 创建 React 管理后台

- Input: “创建一个 React + TypeScript 的内部管理后台项目。”
- Steps: 安装 superpowers 和 vercel-react-best-practices；读取 universal、frontend、react；优先用 Rsbuild、Tailwind、Biome、pnpm；补齐 docs、`.nvmrc`、`.npmrc`、严格 TypeScript。
- Acceptance: 目录和文件名是 kebab-case；状态和接口数据类型完整；README、需求文档、任务拆分文档、部署文档可追溯。

### Example 2: 创建 Vue 国内业务项目

- Input: “做一个 Vue3 项目，后端接口还没定。”
- Steps: 安装 superpowers 和 vue-best-practices；读取 universal、frontend、vue；先产出需求文档和接口草案；使用 TanStack Query 管理接口状态；Vue template 使用短横线组件名。
- Acceptance: 文档先于复杂实现；组件拆分清晰；`.npmrc` 配置国内源；没有 any 和未类型化接口状态。

### Example 3: 创建 Node.js API 服务

- Input: “新建一个 Node.js 后端服务，给前端联调。”
- Steps: 安装 superpowers 和 nodejs-backend-patterns；读取 universal、nodejs；选定框架后建立 OpenAPI/Swagger、Zod schema、接口文档、部署文档。
- Acceptance: 每个接口都有入参和响应类型；Zod 统一校验输入；Swagger 可供前端调试；文档说明鉴权、错误码和部署流程。

## References

- `references/index.md`: 参考文件导航。
- `references/universal.md`: 所有项目必须遵守的通用规范和文档驱动开发要求。
- `references/frontend.md`: 前端项目的技术选型、配置、请求层和质量门禁。
- `references/react.md`: React 项目的状态、组件、接口数据和 useEffect 约束。
- `references/vue.md`: Vue 项目的组件命名、拆分和接口数据约束。
- `references/nodejs.md`: Node.js 后端的 Swagger、Zod、接口类型和联调规范。

## Maintenance

- Sources: 用户提供的团队约束；skills.sh / `npx skills` CLI 的公开安装约定；skill-creator 与 skills-skills 的本地规范。
- Last updated: 2026-04-26
- Known limits: 第三方 skill 的真实 skill 名称和仓库结构可能变化；安装前如失败，先使用 `npx skills add <repo> --list` 或查看对应仓库确认名称。
