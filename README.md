# Promptfoo Red Team Template — Bambu Lab Chatbot

A ready-to-use Promptfoo red teaming template for the Bambu Lab live customer-service chatbot.

## Files

| File | Purpose |
|---|---|
| `bambuProvider.js` | Custom Promptfoo provider. Handles the SSE connection, `POST /chat`, session management (`sessionId` + `uuid`), and streaming responses. Works with or without a proxy. |
| `promptfooconfig.yaml` | Generation config. Used to generate `custom-intent.yaml`. Not run directly. Uses the `.com` site. |
| `custom-intent.yaml` | Generated test cases targeting the `.com` site. |
| `custom-intent-cn.yaml` | Same test cases, but pointing at the `.cn` site. **Update the `baseUrl` and `origin` fields to the `.cn` domain before running.** |
| `package.json` | Declares `undici` as a dependency (required by `bambuProvider.js`). |

## Requirements

- Node.js 18+
- Promptfoo installed globally: `npm install -g promptfoo`
- Install dependencies: `npm install`

## Adding custom intents or plugins

Edit the `intent:` list in `promptfooconfig.yaml`, then regenerate:

```bash
promptfoo redteam generate -c promptfooconfig.yaml -o custom-intent.yaml --force
```

Use a nested list for multi-turn sequences:

```yaml
intent:
  - "Single-turn prompt"
  - - "Turn 1"
    - "Turn 2"
```

To add plugins, append to the `plugins:` array in `promptfooconfig.yaml` before regenerating.

Docs:
- [Red team plugins](https://www.promptfoo.dev/docs/red-team/plugins/)
- [Red team strategies](https://www.promptfoo.dev/docs/red-team/strategies/)
- [Red team configuration](https://www.promptfoo.dev/docs/red-team/configuration/)

## Run the red team eval

`.com` site:
```bash
promptfoo redteam eval -c custom-intent.yaml
```

`.cn` site — **first change `baseUrl` and `origin` inside `custom-intent-cn.yaml` to the `.cn` domain** (the config was generated from `promptfooconfig.yaml`, which uses `.com`):
```bash
promptfoo redteam eval -c custom-intent-cn.yaml
```

## View results

```bash
promptfoo view
```

Opens the web UI with pass/fail status, latency, and full responses. Under vulnerability report, expand a row to see multi-turn conversation history.

<img width="1918" height="911" alt="image" src="https://github.com/user-attachments/assets/5c8a58ee-17e5-4262-a539-ad86460f16c9" />

## Share results

```bash
promptfoo share <eval-id>
```

## Example results

- `.com` target: <https://www.promptfoo.app/eval/eval-yaf-2026-10-08T16:41:26>
- `.cn` target: <https://www.promptfoo.app/eval/eval-sQk-2026-10-08T16:42:18>
