# RapidAPI 机票接口接入说明

## 1) 先旋转（重置）你的 API Key
你在聊天里贴出了 `x-rapidapi-key`，请先去 RapidAPI 控制台重置 key，再把新 key 配到 GitHub Secrets。

## 2) 必配 Secrets
在 GitHub 仓库 `Settings -> Secrets and variables -> Actions` 新建：

- `RAPIDAPI_KEY`：你的 RapidAPI key
- `RAPIDAPI_HOST`：例如 `booking-com15.p.rapidapi.com`

建议同时配置：

- `RAPIDAPI_FLIGHTS_URL`：例如 `https://booking-com15.p.rapidapi.com/api/v1/flights/getMinPrice`
- `RAPIDAPI_CURRENCY`：例如 `CNY`
- `RAPIDAPI_EXTRA_PARAMS`：额外参数 JSON 字符串，例如 `{"adults":1,"cabinClass":"ECONOMY"}`

## 3) 关于你给的 endpoint
你给的是：`/api/v1/flights/getFlightDetails?currency_code=AED`。
这个 endpoint 往往需要一个具体 flight token/id 才能返回单个航班详情，不适合直接做“搜最低价”。
优先使用“searchFlights / search”类接口更适合告警场景。

## 4) 代码行为
`monitor.py` 会优先尝试 RapidAPI：
- 若配置了 `RAPIDAPI_KEY + RAPIDAPI_HOST`，就调用接口并解析 `data.flights`
- 若未配置或无结果，返回空列表（不会触发告警）

## 5) 首次联调建议
先把 `MOCK_PRICE_MODE=false`，手动 `Run workflow` 一次：
- 成功 + 有数据：会进入阈值判断
- 成功 + 无提醒：检查 API 是否真的返回 `data.flights`


## 6) 你发的 getMinPrice 可用吗？
可以，这个 endpoint 比 getFlightDetails 更适合做价格监控。

示例参数：
- fromId: `SHA.AIRPORT`
- toId: `FUK.AIRPORT`
- cabinClass: `ECONOMY`
- currency_code: `AED` 或 `CNY`

代码已默认按 `{IATA}.AIRPORT` 组装 fromId/toId，并兼容 getMinPrice 的 `minPrice` 响应字段。
