# AGENTS.md

给在这个仓库里干活的 agent 看的。README 写给人；这里只写改代码前必须知道、但从代码里不容易看出来的事。

这个仓库是从 `kingwhiiitee/Coach`（原 `BetterAI`）拆出来的，只放 precise-edit。

## 仓库里有什么

| 目录 | 内容 |
| --- | --- |
| `plugins/precise-edit/` | 技能 + PreToolUse 钩子 `scripts/guard.mjs`，拦截整文件重写 |
| `platforms/` | 各平台可直接粘贴的指令；`platforms/codex/*` 和 `platforms/claude-code/*` 会被 `scripts/install.mjs` 安装 |
| `scripts/` | `build.mjs` 打包 `dist/`，`install.mjs` 装到本机 Codex / Claude Code |
| `tests/` | `node --test`，覆盖 guard |

## 常用命令

CI 在 Linux/macOS/Windows × Node 20/22/24 上跑这三条：

```bash
npm test
node scripts/build.mjs
node scripts/install.mjs --all --dry-run
```

## 必须遵守的约定

**提交**：直接提交并推送到 `main`，不开 PR。推送前跑上面的检查，先 `git pull --rebase`。

**零依赖**：只能用 Node 内置模块（Node ≥ 18），并且要在 Windows 上也能跑（路径用 `path`，别假设 `/`）。`build.mjs` 产物要字节可复现：不要引入时间戳、随机数或机器相关内容。

**规则文本有多份**：`guard.mjs` 的 `POLICY`、`skills/precise-edit/SKILL.md`、`platforms/*` 下各平台的版本。改规则要一起改，并同时升 `plugin.json`、`marketplace.json` 和 `package.json` 里的版本号。

**guard 必须 fail open**：自身出错时退出码 0、无输出，绝不能因为 guard 坏了而拦住正常调用。

**安装标记不能改**：`<!-- BEGIN precise-edit (BetterAI) -->` / `<!-- END … -->` 里的 `(BetterAI)` 是旧仓库名，但用户机器上已有的块靠这串文字定位，改了就卸不掉旧块。

## 注释

只写下一个 agent 读代码时自己看不出来的东西：意图、不变量、坑、跨文件的约束。不要写复述代码的注释、分节标题或改动历史。功能性的注释要保留：lint 指令、Markdown 里的 `<!-- BEGIN precise-edit -->` 标记。
