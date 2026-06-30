# Project Instructions / 项目指令

This file is read by Claude Code when placed at the project root.
把它放在项目根目录，Claude Code 启动时会自动读取。

---

## Language / 语言

Always respond in **Simplified Chinese (简体中文)** unless asked otherwise.
默认用简体中文回复，除非用户明确要求用其他语言。

## Code Style / 代码风格

- Use **4 spaces** for indentation (no tabs).
- Prefer **explicit over clever** — code is read more often than written.
- Keep functions short (<30 lines preferred).
- Comment complex logic; don't comment obvious code.

## Commit Messages / 提交信息

Format: `<type>(<scope>): <subject>`

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Examples:
```
feat(auth): add OAuth2 login flow
fix(api): handle null response in /users endpoint
docs(readme): update install steps for macOS
```

Subject line in English, body can be in Chinese for complex changes.

## Testing / 测试

- Write tests alongside code, not after.
- Run `npm test` (or equivalent) before committing.
- Don't commit failing tests — fix or mark as `.skip` with a TODO.

## Security / 安全

- Never commit secrets, API keys, or credentials.
- Use environment variables for sensitive config.
- If unsure whether something is safe to commit, ask first.

## Common Commands / 常用命令

| 操作 | 命令 |
|---|---|
| 启动开发服务器 | `npm run dev` |
| 跑测试 | `npm test` |
| 跑 lint | `npm run lint` |
| 构建生产包 | `npm run build` |

（请根据你的项目修改上面的命令）

## Do NOT / 不要

- Don't modify `package-lock.json` / `yarn.lock` manually.
- Don't add new dependencies without asking first.
- Don't rewrite existing code style — match the surrounding code.
- Don't print sensitive data (tokens, PII) in logs.
