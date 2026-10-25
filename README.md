# signups-data

OSCAR 读书会报名数据（脱敏公开）。

## 为什么独立成库

报名数据原本写在 `website` 仓库的 `static/signups/`，导致**每次有人报名都会触发一次
完整的 GitHub Pages 构建**（约 4 分钟）。报名高峰期会排队、互相取消。

数据独立后：

- `website` 只在**页面内容变更**时构建
- 报名量再大也不触发 CI
- 页面通过 `raw.githubusercontent.com` 直接读取本仓库的 JSON

## 数据格式

`<event>.json`（如 `2026-10-31.json`）：

```json
{
  "event": "2026-10-31",
  "updated": null,
  "signups": [
    { "name": "称呼", "dinner": "yes", "signup_no": "OSCAR-XXXXXX", "signed_at": "..." }
  ]
}
```

**只有脱敏字段**：`name` / `dinner` / `signup_no` / `signed_at`。

联系方式与留言**不在本仓库**，单独存放在内网服务器的
`/home/lee/oscar-signups/<event>.jsonl`（JSONL，追加写，权限 700）。

## 活动后清理

```bash
bash scripts/purge-signups.sh <event> --force   # 在 website 仓库执行
```
