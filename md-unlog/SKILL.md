---
name: md-unlog
disable-model-invocation: true
description: '会话 Markdown 镜像停止协议参考文档。运行时由 pi 扩展（~/.pi/agent/extensions/md-log.ts）静默处理：「停止记录 / 停止镜像 / 结束会话日志」触发，仅 toast `📝 已停止记录`。模型无需执行本技能，切勿自行播报或调用。'
---

# md-unlog：会话 Markdown 镜像（参考文档）

> 运行机制：由 pi 扩展 `~/.pi/agent/extensions/md-log.ts` 统一接管。本文件仅描述停止时对日志文件所做的修改，供阅读与排障，**不作为运行时执行指令**。

## 停止操作（扩展静默执行）

1. 定位活动日志：读 `.sessions/.md-log-active`；不存在 → 在 `.sessions/` 下找最近修改、末尾无结束标记的 `.md` 文件
2. 在日志末尾追加：

   ```markdown
   ---

   *会话记录结束于 <YYYY-MM-DD HH:MM:SS>*
   ```

3. 删除状态文件 `.sessions/.md-log-active`
4. 改写该文件头部会话日志状态为 `已停止`
5. 用户可见输出仅为 toast：`📝 已停止记录`

## 用户可见输出

| 时机 | 唯一可见内容 |
|---|---|
| 停止 | toast：`📝 已停止记录` |
| 停止后 | 任何轮次都不得再向该文件写入 |

## 相关

- 开启技能：md-log（同为参考文档，扩展统一处理）
- 扩展源码：`~/.pi/agent/extensions/md-log.ts`（命令 `/md-log off`）