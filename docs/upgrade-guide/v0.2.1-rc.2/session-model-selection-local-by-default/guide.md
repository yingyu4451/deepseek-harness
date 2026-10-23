---
kind: upgrade-guide
description: "Session model switches no longer replace the deployment default unless the request sets saveDefault."
---

# Session model selection save

English | [中文](guide.zh.md)

## Change

`SessionSelectModelRequest` adds optional `saveDefault`. `session.selectModel` now installs the accepted selection for that Session only; it saves the selection as the deployment default (`agent-default-model`) only when `saveDefault` is `true`. Previously every accepted selection also replaced the default, so an integration that switched its own Session — for example an IM bot applying its per-bot model — silently changed the model that every later Session started from. The Web GUI's `/model` popup and composer model control pass `saveDefault: true` and keep choosing the deployment default.

## Migration

1. Add `saveDefault: true` to every `session.selectModel` call whose selection should also become the default for later Sessions — a model picker that a user treats as the deployment default.
2. Leave the field absent in plugins and integrations that control only their own Session; they no longer rewrite the default.
3. Confirm the behavior: switch a Session with `saveDefault: true`, create a new Session, and check that it starts from the switched selection; repeat without the flag and check that the new Session keeps the previous default.
