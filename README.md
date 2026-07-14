# Team Skills

这个仓库用于存放团队可复用的 AI skills。

## 安装

安装需要的 Skill：

```bash
npx skills add https://github.com/<org>/<repo> --skill web-scaffold
npx skills add https://github.com/<org>/<repo> --skill lean-code
npx skills add https://github.com/<org>/<repo> --skill maintaining-requirement-traceability
```

将 `<org>/<repo>` 替换为本仓库发布后的 GitHub 地址。

## 当前 Skills

- `web-scaffold`: 创建或按现有约定维护 TypeScript 前端与 Node.js 项目；既有项目禁止无关迁移，新项目才应用团队默认，并要求功能、修复和重构采用 TDD。
- `lean-code`: 审查和精简自造轮子、过度抽象、巨型文件及不必要依赖。
- `maintaining-requirement-traceability`: 用技术栈无关的稳定锚点连接需求来源、当前实现和验证证据，不强制引入集中索引或重型 Spec 流程。

名称迁移：原内部名称 `$team-project-scaffold` 已与目录名统一为 `$web-scaffold`，旧提示请同步更新。

## 目录约定

每个 skill 放在 `skills/<skill-name>/` 下，并至少包含：

```text
skills/
  <skill-name>/
    SKILL.md
```

复杂规范拆到 `references/`，避免把所有内容塞进 `SKILL.md`。
