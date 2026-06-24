# Node.js

Node.js 后端项目读取本文件。本文件只记录团队差异化约束；Node.js 后端通用模式交给上游 skill。

## Codex 后端使用边界

后端项目默认把 Codex 当成受限执行者、审稿人和测试补充者，不当成可自主端到端交付大需求的开发者。原因是后端通常包含状态、数据、权限、事务、兼容性、历史逻辑和外部系统语义；任何 AI 改动都必须以保持既有行为为第一优先级。

适合交给 Codex 的后端任务：

- 只读分析代码、梳理调用链和影响面。
- 写小范围 bugfix。
- 补单元测试、业务规则测试、接口契约测试。
- 解释历史代码和输出风险点。
- 做 diff review。
- 生成 migration、API 兼容性或发布风险清单。
- 找重复代码，但不直接重构。

不适合直接交给 Codex 自主完成：

- 大需求端到端实现。
- 跨服务或跨模块重构。
- 状态机重写。
- 权限体系调整。
- 交易、支付、结算、库存等核心逻辑大改。
- 数据库 schema 大改。
- 消息队列语义、event、webhook 语义调整。
- 性能优化时顺手改业务逻辑。
- “顺便把代码整理一下”。

高风险任务如果必须使用 Codex，先要求只分析不改代码，并输出需求理解、涉及文件、既有行为、最小方案、测试计划、风险和回滚说明。实现阶段必须只修改批准过的文件。

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

## 后端任务合同

每个后端改动都必须先写成最小差异合同，再开始实现。合同至少包含：

```md
任务：修复或实现的单一目标。

范围：
- 允许修改的服务、目录和文件。
- 禁止修改的模块、接口、数据结构和依赖。
- 是否允许新增 migration、依赖或配置。

现有行为必须保持：
- 权限规则。
- 状态机规则。
- API request / response 字段。
- 业务错误码。
- 老客户端兼容行为。
- 幂等、事务、缓存、消息语义。

目标行为：
- 明确列出新增或修复后的可观察行为。
- 写清失败路径和边界条件。

完成条件：
- 至少一个失败前会失败、修复后会通过的测试。
- 相关现有测试通过。
- 输出变更文件列表、每个文件修改原因和剩余风险。
```

默认限制一次 AI 后端改动：

- 1 个业务目标。
- 1 个服务或清晰边界内的一个模块。
- 3 到 8 个文件。
- 100 到 300 行 diff。
- 有回滚方案。

超过以上范围时，拆成多个 PR 或先做人审方案。

## 接口类型

- 请求和响应类型必须显式。
- service 层返回类型不要依赖隐式推导到 API 边界。
- 分页、列表、详情、创建、更新、删除接口使用统一响应模型。
- 错误响应保持一致，至少包含错误码、错误消息、追踪 ID 或请求 ID。

## 后端测试策略

后端测试不能只由实现者自行发明验收标准。验收场景应优先由人类或需求文档给出，Codex 只能根据这些场景补测试和实现。

最小测试分层：

- 业务规则测试：状态机、金额计算、权限判断、幂等逻辑。
- 接口契约测试：request / response 字段、错误码、分页和老客户端兼容。
- 集成回归测试：数据库事务、消息队列、缓存一致性、外部服务失败。

实现前优先写反例测试：

```md
先不要实现。
请根据需求找出当前代码应该失败的最小测试用例。
只新增测试，不改业务代码。
测试应体现真实 bug 或缺失行为。
```

反例测试必须先运行并确认失败原因正确，再允许实现。测试不应该只验证 mock 调用次数，而要验证真实业务结果、持久化结果、错误响应或外部事件语义。

## 前后端联调

- Swagger UI 或 OpenAPI JSON 需要在 dev/test 环境可访问。
- README 或 `docs/api.md` 写明本地如何启动 API 文档。
- CORS、鉴权 token、cookie、代理配置要写清楚。
- 如果接口尚未实现，先产出 OpenAPI 草案或 mock 约定，供前端并行开发。

## AI PR 检查清单

后端 PR 合并前，Codex 可以先自查，但必须由人类核对：

```md
需求理解：
- [ ] 是否只实现了明确需求？
- [ ] 是否有未确认假设？
- [ ] 是否改了需求外逻辑？

变更范围：
- [ ] 是否有无关重构？
- [ ] 是否有无关格式化？
- [ ] 是否改了公共 API？
- [ ] 是否改了数据库结构？
- [ ] 是否新增依赖？

后端风险：
- [ ] 权限校验是否保持？
- [ ] 事务边界是否正确？
- [ ] 幂等是否正确？
- [ ] 并发场景是否考虑？
- [ ] MQ / event / webhook 是否兼容？
- [ ] 缓存是否失效正确？
- [ ] 错误码是否兼容？
- [ ] 老数据是否兼容？

测试：
- [ ] 是否有失败前会失败的测试？
- [ ] 是否覆盖异常路径？
- [ ] 是否覆盖边界值？
- [ ] 是否覆盖旧行为不变？
- [ ] 是否不是只测 mock？
```

## 交付检查

- 已安装或已提示安装 nodejs-backend-patterns。
- Swagger/OpenAPI 已接入。
- 每个接口有入参和响应类型。
- Zod 统一校验输入。
- 校验错误和业务错误结构统一。
- `docs/api.md` 或 Swagger 能支持前后端联调。
- 后端任务合同已写明范围、禁止项、现有行为和完成条件。
- 高风险改动已先输出只读分析、风险点和回滚说明。
- 至少有一个反例测试或明确说明为何本次不适用。
- AI PR 检查清单已完成。
- 新增文件和目录名符合 kebab-case。
