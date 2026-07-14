# React

React 项目读取本文件。本文件只记录团队差异化约束；React 通用最佳实践交给上游 skill。

## 按需 Skill

涉及复杂性能、渲染或组件设计，并且当前环境没有等价能力时再安装：

```bash
npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices
```

如果安装失败，使用：

```bash
npx skills add https://github.com/vercel-labs/agent-skills --list
```

确认 skill 名称后再安装。安装失败不阻断基础项目初始化。

## Props 与状态

- props 只允许传递给 pure component。
- 业务组件不推荐多层 props 透传。
- 业务状态优先用 Zustand 或 Jotai 管理。
- 状态设计先定义类型，再实现 store、atom、selector 或 action。
- 不要把远端接口缓存、表单临时状态、页面 UI 状态混在一个大 store 里。

## 接口数据

- 接口数据管理使用 `@tanstack/query`。
- 服务端状态优先放在 query/mutation 中，不要复制到全局 store。
- Zustand 或 Jotai 可以管理 UI 状态、筛选条件、跨页面草稿，也可以结合业务场景使用相关插件。
- query key 必须结构化、类型化，避免裸字符串散落。
- mutation 成功后的缓存更新、失效和错误处理要明确。

## useEffect

- 尽可能避免大量 `useEffect`。
- 不用 `useEffect` 做可以通过事件、派生状态、query、路由加载器或组件组合解决的事情。
- 必须写 `useEffect` 时，把不同副作用拆开。
- 不要把请求、状态同步、DOM 订阅、埋点混在同一个 effect。
- effect 依赖参数尽可能拆分、稳定化，并解释复杂依赖的原因。

## 组件拆分

- 组件尽可能拆分。
- 能做成 pure component 的都拆出去，由父级引用。
- pure component 接收明确 props，不读取业务 store，不发请求，不包含副作用。
- 业务组件负责组合状态、接口数据和动作。
- 单个组件过长时，优先按职责拆分，而不是仅按视觉区域机械拆分。

## 推荐分层

```text
src/
  app/
  pages/
  features/
  entities/
  shared/
    api/
    ui/
    model/
    lib/
```

目录职责：

- `src/app/`: React 应用装配层，例如 router、query client、Zustand/Jotai Provider、全局错误边界、应用初始化。
- `src/pages/`: 路由页面层，负责页面布局和模块组合，不直接堆叠大量请求、状态和 UI 细节。
- `src/features/`: 用户可感知的业务动作模块，例如登录表单、用户禁用、订单创建、报表导出。
- `src/entities/`: 领域实体模块，例如 user、order、product 的类型、实体展示组件、实体级 hooks 和格式化逻辑。
- `src/shared/`: 与业务无关的共享代码，必须保持低耦合，避免反向依赖 pages/features/entities。
- `src/shared/api/`: API client、TanStack Query hooks、query key 工厂、请求/响应类型。
- `src/shared/ui/`: pure component 和通用 UI 组件，只接收 props，不读取业务 store，不直接发请求。
- `src/shared/model/`: 跨模块共享的轻量状态模型、基础类型、通用 store/atom；业务私有状态应留在对应 feature。
- `src/shared/lib/`: 通用 hooks、工具函数、格式化函数、浏览器能力封装。

保持文件名 kebab-case，例如 `user-profile-card.tsx`。组件导出名可以是 PascalCase，这是 TypeScript/React 代码符号，不是文件名。

## 交付检查

- 任务达到按需条件时已加载 vercel-react-best-practices；普通初始化无需安装或提示。
- pure component 和业务组件边界清晰。
- 没有无意义 props drilling。
- 远端数据由 TanStack Query 管理。
- Zustand/Jotai 只承载适合的业务或 UI 状态。
- useEffect 数量和职责可解释。
- 新增组件文件名符合 kebab-case。
