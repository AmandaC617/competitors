# 改版說明

## 概要
- 新增「市場聚焦」卡片，可切換目前預設的分析區域（台灣／東南亞／日本／美國），並同步更新畫面上顯示的選擇，使得 AI 比較思考更貼近實際市場。
- 將「競品比較矩陣」的欄位依「核心體驗」「市場/行銷」「資源能力/策略彈性」三大區塊重新分組，添加顏色標示與描述文字，提升辨識力；同時每格顯示該維度的來源與量化指標。
- `fetchAndAnalyzeData` 的 prompt 現在會帶入選定的市場區域，並要求 AI 回傳 `analysisSources` / `quantifiedMetrics` / `regionTag`，前端會用 tooltip、表格小字與 radar chart 呈現這些資訊。
- 新增「來源驗證」按鈕與 modal、競品公開資訊卡片，並加入「分析快照」列表可儲存/重播過往分析紀錄，讓團隊能快速回顧與比較。

## 新增功能
- `market-focus-card`：顯示目前市場區域並附下拉選單，方便在分析前先定義焦點地區，並同步更新提示說明。
- `comparisonSourcesData` + `publicInfoSourcesData`：渲染來源說明與公開資訊卡片，明列可參考的資料類型（內部文件、官網、App 評分、媒體報導等）與 tooltip。
- `renderComparisonTable()`：三大維度分組、每格附加來源 / 量化指標；`renderMetricComparisonChart()` 往 `metric-comparison-chart` canvas 渲染 radar chart，輔助呈現更新頻率、社群聲量與 App 評分的量化比較。
- `source-validation-modal`：一鍵開啟、列出常見公開資訊連結與建議，保留連結供查證；工具提示延伸至表格與卡片使用相同視覺語彙。
- `analysis-snapshots`：新增「儲存快照」按鈕、快照列表卡片與重播 / 刪除行為，快取最近 5 筆分析並支援重播後自動重新分析，提升跨期觀察效率。

## 資料與更新建議
- 若 AI 回傳的 `analysisSources` 與 `quantifiedMetrics` 無法直接對應，仍可補上 `brand.analysis[`${key}Source`]` 或 `analysis.quantifiedMetrics`，前端已提供 helper 以回退資料，並透過 tooltip 暗示「來源：...」。
- 「來源驗證」 modal 的外部連結應由團隊補上實際型號/競品 URL；可與像 `https://example.com/competitor` 同步，或直接滙入 `publicInfoSourcesData` 來貼上真實 link。
- 快照資料保存在 localStorage，需留意容量（目前保留最近 5 筆）與清除策略；未來可導出 JSON 或同步至後端。

## 測試
- ✅ API key + 「開始深度分析」流程（包含 region 選擇帶入 prompt、loader 提示、錯誤框）。
- ✅ 「來源驗證」按鈕與 modal（確認連結、tooltip、關閉）。
- ✅ 「儲存快照」按鈕 + 快照卡片列表（重播會回填欄位並重新分析、刪除會更新列表）。
- ✅ 競品比較矩陣（來源/量化欄位）與量化 radar chart（資料有數值時正常顯示）。
- ⚠️ 手動確認 charts、alert 等 UI 在不同 region 下仍能正常渲染；尚無自動化測試。

