# 邮箱接收地址配置说明（GitHub Actions）

你问的“发到哪个邮箱”，在当前实现里由环境变量 **`ALERT_EMAIL_TO`** 决定。

## 1) 代码里是怎么读的
`monitor.py` 的 `send_email()` 会读取下面变量：

- `SMTP_HOST`
- `SMTP_PORT`
- `SMTP_USER`
- `SMTP_PASS`
- `SMTP_FROM`（可选，默认同 `SMTP_USER`）
- `ALERT_EMAIL_TO`（收件人邮箱）

其中，真正决定“发给谁”的就是：`ALERT_EMAIL_TO`。

## 2) 在哪里配置
在 GitHub 仓库中配置：

1. 打开仓库 → `Settings`
2. 左侧 `Secrets and variables` → `Actions`
3. 点击 `New repository secret`
4. 新建：
   - Name: `ALERT_EMAIL_TO`
   - Secret: 你的收件邮箱（例如 `you@example.com`）

## 3) 多收件人怎么写
如果你希望发给多人，建议先用一个邮箱组/邮件列表地址；
当前代码按单收件人变量处理，默认最稳。

## 4) 工作流如何注入
`.github/workflows/flight_alert.yml` 已经把 `ALERT_EMAIL_TO` 传给程序：

```yaml
ALERT_EMAIL_TO: ${{ secrets.ALERT_EMAIL_TO }}
```

## 5) 快速自检
- 已配 `ALERT_EMAIL_TO`：会发到你设置的邮箱
- 未配 `ALERT_EMAIL_TO`：`monitor.py` 会报缺少变量错误，任务失败（便于你及时发现）
