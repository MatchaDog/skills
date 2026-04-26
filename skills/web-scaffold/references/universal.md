# Universal

所有项目都必须执行本文件。这里定义跨技术栈的基础约束、文档驱动开发流程和团队知识库友好的交付标准。

## 基础安装

所有安装本 skill 的项目，先安装 superpowers：

```bash
npx skills add https://github.com/obra/superpowers
```

涉及中文文档编写、润色、统一语气或面向企业知识库沉淀时，安装 humanizer-zh：

```bash
npx skills add https://github.com/op7418/humanizer-zh --skill humanizer-zh
```

使用已有成熟 skill 时，只写明安装命令和协作边界，不重复编写对方已经维护的详细内容。

## 命名规范

- 文件和文件夹命名必须使用 kebab-case。
- 不允许出现大写字母、PascalCase、camelCase、snake_case。
- 示例：使用 `user-profile.tsx`，不要使用 `UserProfile.tsx`、`userProfile.tsx`、`user_profile.tsx`。
- 框架约定冲突时，优先寻找框架支持的 kebab-case 写法；确实无法避免时，在文档中记录原因。

## 注释规范

- 注释默认使用中文编写，便于团队知识传承、Code Review 和企业内部知识库检索。
- TypeScript、JavaScript、Vue、React、Node.js 项目优先使用 JSDoc/TSDoc 风格注释。
- 公共函数、公共类型、导出的 hooks、store、API client、service 方法、复杂组件 props 必须补充 JSDoc/TSDoc。
- 注释优先解释“为什么这样做”“业务规则是什么”“边界条件是什么”，不要重复代码已经能表达的“做了什么”。
- 复杂业务判断、兼容逻辑、权限规则、错误码映射、缓存策略、重试策略需要写中文注释。
- 禁止为了满足形式写逐行注释；简单变量赋值、显而易见的 JSX/模板结构不需要注释。
- 临时方案必须用 `TODO`、`FIXME` 或 `HACK` 标记，并写清楚原因、后续处理方式和关联需求或任务。

JSDoc/TSDoc 示例：

```ts
/**
 * 根据当前用户权限过滤可见菜单。
 *
 * 业务约束：
 * - 超级管理员可以看到全部菜单。
 * - 普通用户只能看到已授权且未隐藏的菜单。
 *
 * @param menus 原始菜单树
 * @param permissions 当前用户权限码集合
 * @returns 过滤后的菜单树
 */
export function filter-visible-menus(
  menus: MenuItem[],
  permissions: PermissionCode[],
): MenuItem[] {
  // ...
}
```

## 文档驱动开发

任何项目都需要先建立文档上下文，再开始复杂实现。文档不是形式化材料，而是用于追溯需求、对齐接口、沉淀知识库和降低人员流动成本的工程资产。

中国传统互联网研发环境里，需求、接口、排期、联调、上线、复盘经常跨产品、设计、前端、后端、测试、运维和管理层流转。文档驱动开发能让 AI 和团队成员共享同一套上下文，减少口头约定丢失。

## 推荐文档目录

根据项目规模建立 `docs/`。小项目可以合并文件，但不能缺少对应信息。

```text
docs/
  requirements.md
  api.md
  task-breakdown.md
  deployment.md
  user-guide.md
  decisions.md
  changelog.md
README.md
```

## 必备文档

### 需求文档

`docs/requirements.md` 至少包含：

- 背景和业务目标。
- 用户角色和主要场景。
- 功能范围和不做什么。
- 关键流程。
- 权限、数据、性能、安全、合规等非功能要求。
- 验收标准。
- 未决问题和决策记录链接。

### 接口文档

`docs/api.md` 或 Swagger/OpenAPI 至少包含：

- 接口用途。
- 请求方法和路径。
- 鉴权方式。
- 入参类型、必填性、默认值、边界条件。
- 响应结构。
- 错误码和错误响应。
- 示例请求和示例响应。
- 前后端联调注意事项。

### 任务拆分文档

`docs/task-breakdown.md` 至少包含：

- 里程碑。
- 任务列表。
- 每个任务的负责人或执行角色。
- 依赖关系。
- 验收标准。
- 风险和阻塞项。

任务拆分要能直接指导 AI 或工程师执行，不写“优化页面”这类不可验收任务。写成“实现登录表单校验，覆盖手机号、验证码、服务端错误三类状态”。

### README.md

根目录 `README.md` 至少包含：

- 项目简介。
- 技术栈。
- 本地启动。
- 环境变量。
- 常用脚本。
- 目录结构。
- 文档入口。
- 部署入口。

### 部署文档

`docs/deployment.md` 至少包含：

- 环境说明：local、dev、test、staging、prod。
- 构建命令。
- 发布步骤。
- 回滚方案。
- 环境变量和密钥管理。
- 日志、监控、告警入口。
- 常见故障处理。

### 使用文档

`docs/user-guide.md` 至少包含：

- 面向最终用户或运营人员的主要流程。
- 常见操作步骤。
- 权限说明。
- 常见问题。
- 截图或示例数据，如项目允许。

### 决策记录

`docs/decisions.md` 记录重要技术和产品决策：

- 决策日期。
- 背景。
- 可选方案。
- 最终选择。
- 取舍原因。
- 影响范围。

## 文档写作要求

- 默认中文编写。
- 面向企业内部知识库，避免只有作者能懂的缩写。
- 每个文档都要有清晰标题、更新时间、适用范围。
- 需求和接口发生变化时，同步更新文档，不允许只改代码。
- AI 生成文档时，使用 humanizer-zh 做中文表达校正，但保留技术准确性。

## 项目交付门禁

交付前检查：

- 目录和文件名符合 kebab-case。
- 关键公共 API、复杂业务逻辑和临时方案有中文 JSDoc/TSDoc 或中文注释。
- 已安装或已明确提示安装 superpowers。
- 必备文档存在，并且不是空模板。
- README 指向 docs 下的关键文档。
- 重要需求、接口、部署步骤可追溯。
- 所有偏离本规范的地方写入 `docs/decisions.md`。
