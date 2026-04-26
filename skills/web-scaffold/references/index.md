# Team Project Scaffold References

按项目类型只读取需要的文件，避免把无关规范塞进上下文。

## 文件导航

- `universal.md`: 所有项目都必须读取。包含目录命名、基础 skill、文档驱动开发、文档目录和验收标准。
- `frontend.md`: 有浏览器端或前端工程时读取。包含 TypeScript、Tailwind、Rsbuild、Biome、请求层、包管理、国内源等规范。
- `react.md`: React 项目读取。包含 vercel-react-best-practices 安装、props、状态管理、TanStack Query、useEffect 和组件拆分。
- `vue.md`: Vue 项目读取。包含 vue-best-practices 安装、template 命名、import 命名、组件拆分、TanStack Query。
- `nodejs.md`: Node.js 后端读取。包含 nodejs-backend-patterns 安装、Swagger/OpenAPI、Zod 输入校验、接口类型和联调。

## 使用顺序

1. 永远先读 `universal.md`。
2. 前端项目继续读 `frontend.md`。
3. React、Vue、Node.js 根据技术栈读取对应文件。
4. full-stack 项目读取 `universal.md`、`frontend.md`、对应前端框架文件和 `nodejs.md`。
5. 如果用户明确指定了与本规范不同的技术方案，记录偏离原因，并在文档中写明。

## 第三方 Skill 原则

当本仓库引用了已有成熟 skill，只做安装和协同说明，不复制对方的完整规范。这样可以减少重复维护，并让团队随着上游 skill 更新获得收益。
