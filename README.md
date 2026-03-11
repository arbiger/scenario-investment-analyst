# 情境投資分析師 (Scenario Investment Analyst)

一個結合預測市場、宏觀流動性和估值模型的投資情境分析系統。

## 定位

當用戶提出：
- 假設性問題（「如果伊朗停火會怎樣？」）
- 市場情況（「現在市場怎麼樣？」）
- 投資建議（「這檔股票可以買嗎？」）

自動進行情境分析並給出投資建議。

## 執行流程

```
Step 1: 理解問題
    │
    └─ 產出多種可能情境 (情境A, 情境B, ...)

Step 2: 情境 → Polymarket 搜尋
    │
    ├─ 根據情境產生關鍵詞
    ├─ 搜尋相關市場 (web_search / browser)
    └─ 輸出市場預期機率

Step 3: Macro Liquidity 宏觀環境
    │
    ├─ Fed 縮表/流動性
    ├─ SOFR 利率
    └─ 風險指標

Step 4: 估值分析
    │
    ├─ 有持股 → 跑 PEG + DCF (Valuation Calculator)
    │
    └─ 無持股 → 用戶指定或熱門股票

Step 5: 輸出建議
    │
    ├─ 各情境分析
    ├─ 受益/受害產業
    └─ 詢問：「要深入分析持股嗎？」

Step 6: 可選 - 深度分析
    │
    └─ 用戶同意 → Tech Earnings Deepdive
```

## Dependencies

此技能依賴以下 Skills：

| Skill | 用途 | GitHub/Source |
|-------|------|---------------|
| **Polymarket Analysis** | 預測市場搜尋與分析 | ClawHub: `polymarket-analysis` |
| **Macro Liquidity** | 宏觀流動性監控 | Day1Global (需自行安裝) https://github.com/star23/Day1Global-Skills |
| **Valuation Calculator** | PEG + DCF 估值 | https://github.com/arbiger/valuation-calculator |
| **Tech Earnings Deepdive** | 深度財報分析 | Day1Global (需自行安裝) https://github.com/star23/Day1Global-Skills |
