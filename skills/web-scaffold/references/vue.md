# Vue

Vue 项目读取本文件。本文件只记录团队差异化约束；Vue 通用最佳实践交给上游 skill。

## 必装 Skill

Vue 项目默认先执行：

```bash
npx skills add https://github.com/hyf0/vue-skills --skill vue-best-practices
```

如果安装失败，使用：

```bash
npx skills add https://github.com/hyf0/vue-skills --list
```

确认 skill 名称后再安装。

## 组件命名

Vue 组件在 template 里只能使用短横线写法：

```vue
<template>
  <menu-icon />
</template>
```

组件引入的地方必须使用驼峰或 PascalCase import：

```ts
import { MenuIcon } from "lucide-vue-next";
```

不要在 template 里写 `<MenuIcon />`。template 统一用 `<menu-icon />`，提升模板一致性和可读性。

## 组件拆分

- 组件尽可能拆分。
- 能做成 pure component 的都拆出去，由父级引用。
- pure component 只接收明确 props 和 emit，不直接请求接口，不读写全局业务状态。
- 容器组件负责组合接口数据、状态、权限和业务动作。
- 不要把表单、表格、弹窗、请求和权限逻辑全部写进一个 `.vue` 文件。

## 接口数据

- 接口数据管理使用 `@tanstack/query` 的 Vue 版本。
- query key 必须结构化、类型化。
- mutation 的成功、失败、缓存失效、乐观更新要明确。
- 不要把服务端状态复制到 Pinia 或其他全局状态里，除非有清晰业务理由。

## 推荐目录

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

- `src/app/`: Vue 应用装配层，例如 app 创建、router、query client、全局插件、全局样式和应用初始化。
- `src/pages/`: 路由页面层，负责页面结构和模块组合，避免把接口、状态、表单、弹窗全部写在一个页面文件里。
- `src/features/`: 用户动作或业务能力模块，例如登录、创建订单、导出报表、修改状态。
- `src/entities/`: 领域实体模块，例如 user、order、product 的类型、实体展示组件、实体级 composables 和格式化逻辑。
- `src/shared/`: 与业务无关的共享代码，不能依赖具体页面或具体 feature。
- `src/shared/api/`: API client、TanStack Query composables、query key 工厂、请求/响应类型。
- `src/shared/ui/`: 可复用 pure component，只通过 props/emit 通信，不直接发请求，不读写业务状态。
- `src/shared/model/`: 跨模块共享的基础状态模型、通用类型、轻量 store；业务私有状态应留在对应 feature。
- `src/shared/lib/`: 通用 composables、工具函数、格式化函数、浏览器能力封装。

文件名保持 kebab-case，例如 `user-profile-card.vue`。

## 交付检查

- 已安装或已提示安装 vue-best-practices。
- template 中组件使用短横线写法。
- import 中组件符号使用驼峰或 PascalCase。
- 远端数据由 TanStack Query 管理。
- pure component 和容器组件边界清晰。
- 新增 `.vue` 文件名符合 kebab-case。
