# 如何判断“机票监控真的在触发”

成功跑完 workflow 只表示“程序执行成功”，不等于“触发了降价提醒”。

现在 `monitor.py` 会在日志里打印两个关键指标：

- `fetched_records=...`：本次抓到了多少条票价
- `alerts_sent=...`：本次发出了多少条提醒

## 1) 先看日志指标
在 Actions -> 该次运行 -> `Run monitor` 日志末尾看：

- `fetched_records > 0`：说明 API 接入成功，拿到了票价
- `alerts_sent > 0`：说明阈值触发并且提醒已发出

## 2) 强制联调（不依赖是否降价）
如果你要验证飞书/邮件通道是否可达，可临时设置：

- `FORCE_ALERT=true`

这会在有票价记录时强制发提醒（仍受去重逻辑影响）。

## 3) 建议的验收顺序
1. `MOCK_PRICE_MODE=false`
2. 确认 RapidAPI secrets 正确
3. 运行 workflow
4. 看 `fetched_records` 是否大于 0
5. 若 `alerts_sent=0`，临时加 `FORCE_ALERT=true` 验证通知链路
6. 验证后把 `FORCE_ALERT` 改回 `false`
