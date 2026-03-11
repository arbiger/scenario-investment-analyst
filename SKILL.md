---
name: scenario-investment-analyst
description: 情境投資分析師 - 結合預測市場、宏觀流動性和估值模型進行投資情境分析。當用戶提出假設性投資問題（如「如果...會怎樣？」）、市場情況詢問、或股票投資建議需求時觸發此技能。支援情境模擬、市場情緒查詢、持股估值分析，並可調用深度財報分析技能。
---

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
| **Macro Liquidity** | 宏觀流動性監控 | Day1Global (需自行安裝) |
| **Valuation Calculator** | PEG + DCF 估值 | https://github.com/arbiger/valuation-calculator |
| **Tech Earnings Deepdive** | 深度財報分析 | Day1Global (需自行安裝) |

## 安裝需求

```bash
# 必須安裝
clawhub install polymarket-analysis

# Valuation Calculator (Python)
pip install yfinance
git clone https://github.com/arbiger/valuation-calculator.git
```

## 使用範例

### 範例 1: 情境假設問題

```
User: 如果伊朗停火會怎樣？

→ Step 1: 產情境
  - 情境A: 停火成功
  - 情境B: 停火失敗

→ Step 2: Polymarket 搜尋
  - 搜尋關鍵詞: "Iran ceasefire", "US Iran peace"
  - 找到: "US x Iran ceasefire by..." market
  - 輸出機率: June 30 前 67%

→ Step 3: Macro
  - 流動性: Fed 每月縮表 ~$600億
  - SOFR: ~4.3%
  - 結論: 流動性偏緊

→ Step 4: 估值
  - 跑持股 PEG + DCF
  - 輸出低估/高估股票

→ Step 5: 輸出建議
  情境A (停火成功):
    - 受益: 航空、能源、旅遊
    - 受害: 國防承包商
  
  情境B (停火失敗):
    - 受益: 防禦股(BTI)
    - 受害: 航空、TSLA

→ Step 6: 詢問
  「要深入分析特定持股嗎？」
```

### 範例 2: 市場情況詢問

```
User: 現在市場流動性怎麼樣？

→ Step 2: Polymarket (可選)
  - 搜尋: "S&P 500" 預測市場

→ Step 3: Macro Liquidity
  - Fed 資產負債表
  - SOFR 利率
  - 逆回購規模
  - MOVE 指數

→ Step 5: 輸出
  - 流動性評級: 緊張/適度/寬鬆
  - 對風險資產影響
```

### 範例 3: 股票投資建議

```
User: NVDA 可以買嗎？

→ Step 2: Polymarket (可選)
  - 搜尋 AI 相關市場情緒

→ Step 3: Macro (可選)
  - 流動性對科技股影響

→ Step 4: 估值
  - 跑 PEG + DCF
  - 輸出公平價值與漲跌空間

→ Step 5: 輸出建議
  - 評估: 低估/合理/高估
  - 風險點
  - 詢問：「要深度分析財報嗎？」
```

## 輸出格式

### 情境分析輸出

```markdown
## 📊 情境分析報告

### 市場預期
| 情境 | 機率 | 來源 |
|------|------|------|
| 停火成功 | 67% | Polymarket |
| 停火失敗 | 33% | 隱含 |

### 宏觀環境
| 指標 | 狀態 | 影響 |
|------|------|------|
| Fed 縮表 | 每月~$600億 | 流動性收緊 |
| SOFR | ~4.3% | 資金成本高 |

### 持股估值
| 股票 | 評估 | PEG | DCF |
|------|------|-----|-----|
| HIMS | 🔥 | 0.08 | -92% |
| BTI | 👍 | 0.77 | +181% |

### 情境A: 停火成功
受益產業: 航空、能源、旅遊
受害產業: 國防

### 情境B: 停火失敗
受益產業: 防禦股(BTI)、醫療
受害產業: 航空、TSLA

---
要深入分析特定持股嗎？
```

## 注意事項

1. **多重情境**：每個問題應產出至少 2 種情境
2. **數據驗證**：市場機率應從 Polymarket 實際搜尋取得
3. **估值結合**：PEG + DCF 綜合評估（避免 PEG 陷阱）
4. **持續詢問**：分析後詢問用戶是否要深入特定股票
5. **風險提示**：最後必須附上風險聲明

## 風險聲明

此分析僅供參考，不構成投資建議。過去表現不代表未來結果。
