# Team Skills

这个仓库用于存放团队可复用的 AI skills。

## 安装

安装团队新项目脚手架规范：

```bash
npx skills add https://github.com/<org>/<repo> --skill team-project-scaffold
```

将 `<org>/<repo>` 替换为本仓库发布后的 GitHub 地址。

## 当前 Skills

- `team-project-scaffold`: 团队新项目脚手架和工程规范，覆盖通用文档驱动开发、前端、React、Vue、Node.js 后端。

## 目录约定

每个 skill 放在 `skills/<skill-name>/` 下，并至少包含：

```text
skills/
  <skill-name>/
    SKILL.md
```

复杂规范拆到 `references/`，避免把所有内容塞进 `SKILL.md`。
