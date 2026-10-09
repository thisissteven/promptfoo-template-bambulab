# Promptfoo 红队测试模板 — Bambu Lab 聊天机器人

[English](README.md)

一个开箱即用的 Promptfoo 红队测试模板，用于测试 Bambu Lab 在线客服聊天机器人。

## 文件说明

| 文件 | 用途 |
|---|---|
| `bambuProvider.js` | 自定义 Promptfoo provider。处理 SSE 连接、`POST /chat` 请求、会话管理（`sessionId` + `uuid`）以及流式响应解析。支持使用代理，也支持直连。 |
| `promptfooconfig.yaml` | 生成配置。用于生成 `custom-intent.yaml`，不直接运行。使用 `.com` 站点。 |
| `custom-intent.yaml` | 生成的测试用例，针对 `.com` 站点。 |
| `custom-intent-cn.yaml` | 相同的测试用例，但指向 `.cn` 站点。**运行前必须将 `baseUrl` 和 `origin` 字段修改为 `.cn` 域名。** |
| `package.json` | 声明 `undici` 依赖（`bambuProvider.js` 需要）。 |

## 环境要求

- Node.js 18+
- 全局安装 Promptfoo：`npm install -g promptfoo`
- 安装依赖：`npm install`

## 添加自定义 intent 或 plugin

编辑 `promptfooconfig.yaml` 中的 `intent:` 列表，然后重新生成：

```bash
promptfoo redteam generate -c promptfooconfig.yaml -o custom-intent.yaml --force
```

多轮对话使用嵌套列表：

```yaml
intent:
  - "单轮提示词"
  - - "第一轮"
    - "第二轮"
```

添加 plugin 时，在 `promptfooconfig.yaml` 的 `plugins:` 数组中追加条目，然后重新生成。

文档：
- [红队 plugin](https://www.promptfoo.dev/docs/red-team/plugins/)
- [红队策略](https://www.promptfoo.dev/docs/red-team/strategies/)
- [红队配置](https://www.promptfoo.dev/docs/red-team/configuration/)

## 运行红队评估

`.com` 站点：
```bash
promptfoo redteam eval -c custom-intent.yaml
```

`.cn` 站点 — **首先在 `custom-intent-cn.yaml` 中将 `baseUrl` 和 `origin` 修改为 `.cn` 域名**（该配置文件由 `promptfooconfig.yaml` 生成，默认使用 `.com`）：
```bash
promptfoo redteam eval -c custom-intent-cn.yaml
```

## 查看结果

```bash
promptfoo view
```

打开 Web UI，查看通过/失败状态、延迟以及完整响应。在漏洞报告中展开某一行，即可查看多轮对话历史。

<img width="1918" height="911" alt="image" src="https://github.com/user-attachments/assets/5c8a58ee-17e5-4262-a539-ad86460f16c9" />

## 分享结果

```bash
promptfoo share <eval-id>
```

## 示例结果

- `.com` 目标：<https://www.promptfoo.app/eval/eval-yaf-2026-10-08T16:41:26>
- `.cn` 目标：<https://www.promptfoo.app/eval/eval-sQk-2026-10-08T16:42:18>
