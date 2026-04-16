# MCP Taiwan Flood

> 台灣水情與防洪資料 MCP Server - 提供淹水潛勢、河川水位、雨量、水庫等即時資料

## 工具清單

| 工具名稱 | 說明 |
|----------|------|
| `get_flood_potential` | 查詢台灣各地區淹水潛勢資料，可依縣市或鄉鎮篩選 |
| `get_river_water_level` | 取得河川水位觀測資料，含警戒狀態 |
| `get_rainfall_data` | 查詢即時雨量觀測資料，可依縣市或測站篩選 |
| `get_flood_warnings` | 取得即時淹水警報，來自 Civil IoT 感測器 |
| `get_reservoir_status` | 取得全台水庫即時水情，含蓄水量與進出水量 |

## 安全徽章

- **資料敏感度**: public（公開資料）
- **權限**: readonly（僅讀取）
- **開源**: 是
- **外部資料源**:
  - https://opendata.wra.gov.tw
  - https://sta.ci.taiwan.gov.tw
  - https://sta.colife.org.tw

## 安裝

```bash
npm install
```

## 使用方式

### Cloudflare Workers 部署

```bash
npx wrangler deploy
```

部署後會得到一個 Worker URL，可透過 JSON-RPC 2.0 協定呼叫。

### MCP 協定流程

1. **initialize** - 初始化連線，取得 server 資訊
2. **tools/list** - 列出所有可用工具
3. **tools/call** - 呼叫特定工具

### 範例請求

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "get_flood_potential",
    "arguments": {
      "county": "臺北市",
      "town": "信義區"
    }
  }
}
```

### Claude Desktop 整合

在 `claude_desktop_config.json` 中新增：

```json
{
  "mcpServers": {
    "taiwan-flood": {
      "url": "https://your-worker.workers.dev"
    }
  }
}
```

## 技術棧

- **框架**: Hono + Cloudflare Workers
- **語言**: TypeScript (strict mode)
- **測試**: Vitest (71 tests, 100% coverage)
- **協定**: MCP (Model Context Protocol) 2024-11-05

## 開發

```bash
# 開發模式
npm run dev

# 執行測試
npm test

# 測試覆蓋率
npm run test:coverage

# TypeScript 檢查
npx tsc --noEmit
```

## 資料來源

- 水利署開放資料平台
- Civil IoT 台灣民生公共物聯網
- 農田水利署資料服務

## License

MIT
