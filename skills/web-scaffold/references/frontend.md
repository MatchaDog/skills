# Frontend

有前端工程时读取本文件。这里定义浏览器端项目的默认技术选型、配置文件、请求封装、类型约束和质量门禁。

## 技术选型优先级

- 优先使用项目已安装或用户指定的组件库完成页面，不重复造轮子。
- 新选组件库时，综合判断活跃度、star 数、维护频率、生态兼容、可访问性、主题能力、需求匹配度。
- 创建新项目时，优先使用 Rsbuild，而不是 Vite。
- 质量检测和格式化优先使用 Biome，而不是 ESLint + Prettier。
- 包管理器优先 pnpm 或 bun，不优先 npm。

## TypeScript

- 所有前端项目必须使用 TypeScript。
- 开启严格模式：`strict: true`。
- 不允许使用 `any`。
- 确实需要 `any` 的场景必须让用户手动介入确认，并记录在代码注释或 `docs/decisions.md`。
- 状态流必须类型驱动：表单状态、接口响应、缓存数据、全局状态都要有明确类型。
- 优先使用 `unknown`、泛型、判别联合、Zod schema 或框架类型推导来替代 `any`。

## CSS

- 统一使用 Tailwind CSS。
- 避免 styled-components、SCSS、Less、Stylus、纯 CSS 等分散方案。
- 如果项目已有成熟样式系统，先说明差异，并按用户或现有代码决策执行。
- Tailwind class 过长时，优先抽成语义化组件，而不是引入新的样式方案。

## 请求库

根据用户需求和后端环境选择：

- axios：偏 XMLHttpRequest，适合需要上传进度、取消请求、较传统拦截器生态的项目。
- ky：偏 fetch，适合轻量现代项目、边缘运行时或希望贴近 Web 标准的项目。

中国国内常见后端场景里，通常需要封装 `src/shared/request.ts` 或同等位置。封装至少包含：

- base URL 读取环境变量。
- 通用请求头。
- token 或会话凭证处理。
- 请求拦截器。
- 响应拦截器。
- 统一错误结构。
- 登录失效处理。
- 业务错误码映射。
- 超时和重试策略，如业务允许。
- 文件上传下载处理，如业务需要。

请求封装必须暴露类型化调用方式。不要让页面组件直接散落 `fetch` 或 `axios` 原始调用。

## HTML 语义化

- 尽可能使用语义化标签：`main`、`section`、`article`、`header`、`footer`、`nav`、`aside`、`form`、`label`、`button`、`table`。
- 避免大量无意义的 `div` 和 `p` 堆叠。
- 语义化标签能提升可访问性，也能让 AI 和团队成员更快理解页面结构。
- 可点击元素使用 `button` 或链接，不用 `div` 模拟。

## package.json

`package.json` 必须注明：

- Node.js 版本：`engines.node`。
- 包管理器版本：`packageManager`。
- 常用脚本：`dev`、`build`、`typecheck`、`lint`、`format` 或项目等价脚本。

示例：

```json
{
  "engines": {
    "node": ">=20.10.0"
  },
  "packageManager": "pnpm@9.15.0",
  "scripts": {
    "dev": "rsbuild dev",
    "build": "rsbuild build",
    "typecheck": "tsc --noEmit",
    "lint": "biome check .",
    "format": "biome format --write ."
  }
}
```

## 根目录配置

根目录需要有 `.nvmrc`，内容与 `package.json` 的 Node.js 版本一致或兼容。

国内项目根目录需要有 `.npmrc`，并配置国内源。默认可使用：

```ini
registry=https://registry.npmmirror.com
```

如果团队使用私有 npm registry，以私有源为准，并在 README 中说明登录方式和只读 token 管理方式。

## 推荐目录

根据框架微调，但保持 kebab-case：

```text
src/
  app/
  pages/
  widgets/
  features/
  entities/
  shared/
    api/
    config/
    lib/
    request.ts
    types/
```

目录职责：

- `src/app/`: 应用入口和全局装配层，例如路由注册、全局 Provider、全局样式入口、应用级初始化。
- `src/pages/`: 页面级模块，一个路由页面对应一个目录或文件，负责组合 feature、widget 和 entity，不承载过多底层业务细节。
- `src/widgets/`: 跨页面复用的业务区块，例如顶部导航、侧边栏、筛选面板、数据概览卡组。
- `src/features/`: 面向用户动作或业务能力的功能模块，例如登录、创建订单、导出报表、修改用户状态。
- `src/entities/`: 领域实体相关代码，例如 user、order、product 的类型、展示组件、实体级工具函数。
- `src/shared/`: 与具体业务领域无关的共享能力，例如通用 UI、请求封装、工具函数、配置、通用类型。
- `src/shared/api/`: 接口声明、API client、query/mutation 封装和接口类型，不直接写页面 UI。
- `src/shared/config/`: 环境变量读取、运行时配置、常量配置，避免配置散落在业务代码里。
- `src/shared/lib/`: 纯工具函数、格式化函数、通用 hooks/composables，不依赖具体业务页面。
- `src/shared/request.ts`: axios 或 ky 的统一请求实例，集中处理 base URL、请求头、拦截器、错误结构和鉴权失效。
- `src/shared/types/`: 跨模块共享的基础类型，例如分页、统一响应、选项项、权限码等。

如果项目已经采用其他架构，例如 Next.js App Router、Vue 官方目录或内部脚手架，以现有约定为准，但仍保持命名、类型和文档要求。

## 质量门禁

交付前至少检查：

- TypeScript 严格模式可用。
- 没有新增 `any`。
- Tailwind 是主要样式方案。
- 请求层统一封装。
- 组件库选择有依据。
- `package.json`、`.nvmrc`、`.npmrc` 存在并一致。
- 运行已有 lint、typecheck、build；如果没有脚本，补齐或说明缺口。
