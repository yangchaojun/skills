---
name: md-unlog
description: Use when the user asks to stop logging, stop mirroring, or end the Markdown session log (md-unlog). Triggers include 停止记录、停止镜像、结束会话日志.
---

# md-unlog：停止会话 Markdown 镜像

## 步骤

1. 定位活动日志：
   - 读 `<cwd>/.sessions/.md-log-active`
   - 不存在 → 在 `.sessions/` 下找最近修改、末尾无结束标记的 `.md` 文件
   - 仍找不到 → 告知用户没有进行中的日志，结束
2. 在日志末尾追加：

   ```markdown
   ---

   *会话记录结束于 <YYYY-MM-DD HH:MM:SS>*
   ```

3. 删除状态文件 `.sessions/.md-log-active`
4. 向用户确认已停止。此后任何轮次都不得再向该文件写入。
5. 改写该文件头部会话日志状态为 `已停止`

## 常见错误

| 错误 | 纠正 |
|------|------|
| 找不到状态文件就报错放弃 | 先执行步骤 1 的回退查找 |
| 停止后仍继续追加 | 结束标记之后禁止任何写入 |
