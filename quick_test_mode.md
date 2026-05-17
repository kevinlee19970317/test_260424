# 不接机票 API 也能触发提醒（测试模式）

你现在没接真实机票 API，是正常的。默认情况下脚本不会抓到任何票价，所以不会提醒。

为了先验证“飞书 + 邮件”链路，`monitor.py` 支持测试模式：

- `MOCK_PRICE_MODE=true`：启用模拟票价
- `MOCK_BASELINE_PRICE=2000`：写入历史均价基线
- `MOCK_PRICE=1200`：当前票价（低于均价就会触发）
- `MOCK_IS_DIRECT=true`：模拟直飞

## 在 GitHub Actions 里怎么配
在 workflow 的 `Run monitor` 里加：

```yaml
env:
  MOCK_PRICE_MODE: "true"
  MOCK_BASELINE_PRICE: "2000"
  MOCK_PRICE: "1200"
  MOCK_IS_DIRECT: "true"
```

跑一次手动任务后，如果 Secrets 正确，你应能收到飞书/邮件提醒。

> 测试结束后把 `MOCK_PRICE_MODE` 改回 `false`，避免持续发送测试消息。
