---
kind: upgrade-guide
description: "会话切换模型不再改写部署默认值，除非请求设置 saveDefault。"
---

# 会话模型选择保存

[English](guide.md) | 中文

## 变更

`SessionSelectModelRequest` 新增可选字段 `saveDefault`。`session.selectModel` 现在只把接受的选择安装到该会话；仅当 `saveDefault` 为 `true` 时，才把该选择保存为部署默认值（`agent-default-model`）。此前每次被接受的选择都会顺带替换默认值，因此只切换自身会话的集成（例如 IM 机器人应用其按机器人保存的模型）会悄然改变之后每个新会话的起始模型。Web GUI 的 `/model` 弹窗与 composer 模型控件会传入 `saveDefault: true`，继续负责选择部署默认值。

## 迁移

1. 对每个同时应成为后续会话默认值的 `session.selectModel` 调用补上 `saveDefault: true`，例如用户视为部署默认值的模型选择器。
2. 只控制自身会话的插件与集成保持省略该字段；它们不再改写默认值。
3. 确认行为：带 `saveDefault: true` 切换会话后新建会话，应看到新会话从切换后的选择开始；去掉该标志重复一次，应看到新会话沿用此前的默认值。
