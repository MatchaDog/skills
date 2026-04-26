# Node.js

Node.js 后端项目读取本文件。本文件只记录团队差异化约束；Node.js 后端通用模式交给上游 skill。

## 必装 Skill

Node.js 后端项目默认先执行：

```bash
npx skills add https://github.com/wshobson/agents --skill nodejs-backend-patterns
```

如果安装失败，使用：

```bash
npx skills add https://github.com/wshobson/agents --list
```

确认 skill 名称后再安装。

## Swagger / OpenAPI

- 一定要接入 Swagger 或 OpenAPI。
- 所有接口的入参和响应都需要类型支持。
- 每个接口都要补充 Swagger/OpenAPI 注释或 schema，方便前后端调试。
- Swagger 文档需要覆盖鉴权、错误码、分页、排序、筛选、文件上传下载等通用约定。
- OpenAPI 输出应能被前端用于生成类型或至少作为联调依据。

接口文档必须明确：

- method 和 path。
- request params、query、body、headers。
- response 成功结构。
- response 错误结构。
- 业务错误码。
- 权限要求。
- 示例请求和响应。

## Zod 输入校验

- Validate input 一定要使用 Zod 统一管理和处理。
- 请求入参 schema、业务 DTO、Swagger/OpenAPI schema 要尽量从同一份类型来源派生，减少漂移。
- 不要在 controller 或 route handler 里散落手写校验。
- 校验失败需要返回统一错误结构，便于前端处理。

示例结构：

```text
src/
  modules/
    user/
      user.schema.ts
      user.service.ts
      user.controller.ts
      user.routes.ts
  shared/
    errors/
    openapi/
    validation/
```

目录职责：

- `src/modules/`: 按业务领域组织后端模块，例如 user、order、auth。每个模块内部维护自己的 schema、service、controller、routes。
- `src/modules/user/`: user 领域示例目录，真实项目中替换为具体业务领域名，并保持 kebab-case。
- `user.schema.ts`: 当前领域的 Zod schema、请求 DTO、响应 DTO 或类型派生入口。
- `user.service.ts`: 业务逻辑层，处理领域规则、事务、外部服务调用，不直接绑定 HTTP 框架。
- `user.controller.ts`: HTTP 控制层，负责读取已校验入参、调用 service、组装响应。
- `user.routes.ts`: 路由注册层，绑定 method/path、中间件、controller 和 Swagger/OpenAPI 注释。
- `src/shared/`: 后端共享能力，必须与具体业务模块保持低耦合。
- `src/shared/errors/`: 统一错误类、错误码、错误响应映射和异常处理中间件。
- `src/shared/openapi/`: Swagger/OpenAPI 初始化、schema 注册、文档路由、公共响应模型。
- `src/shared/validation/`: Zod 校验中间件、通用 schema、请求校验工具和校验错误格式化。

## 接口类型

- 请求和响应类型必须显式。
- service 层返回类型不要依赖隐式推导到 API 边界。
- 分页、列表、详情、创建、更新、删除接口使用统一响应模型。
- 错误响应保持一致，至少包含错误码、错误消息、追踪 ID 或请求 ID。

## 前后端联调

- Swagger UI 或 OpenAPI JSON 需要在 dev/test 环境可访问。
- README 或 `docs/api.md` 写明本地如何启动 API 文档。
- CORS、鉴权 token、cookie、代理配置要写清楚。
- 如果接口尚未实现，先产出 OpenAPI 草案或 mock 约定，供前端并行开发。

## 交付检查

- 已安装或已提示安装 nodejs-backend-patterns。
- Swagger/OpenAPI 已接入。
- 每个接口有入参和响应类型。
- Zod 统一校验输入。
- 校验错误和业务错误结构统一。
- `docs/api.md` 或 Swagger 能支持前后端联调。
- 新增文件和目录名符合 kebab-case。
