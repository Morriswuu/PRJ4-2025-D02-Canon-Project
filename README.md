# InkSight — Canon Printer Filter Flow Analysis
### Canon 印表機墨水過濾器健康分析專案

---

## Project Overview | 專案簡介

Canon 生產型印表機因墨水過濾器（ink filter）的健康狀況而頻繁發生故障與停機。本專案透過歷史過濾流量資料與錯誤記錄，分析故障模式、預測維護需求，並優化服務排程，目標是在不需要現場調查的情況下即可提供資料驅動的維護建議。

Canon production printers experience recurring failures and downtime related to ink filter health. This project analyzes historical filter flow and error data to identify failure patterns, predict maintenance needs, and optimize service schedules — without requiring direct on-site investigation.

---

## Objectives | 分析目標

- - 找出高維護頻率的印表機，建立主動維護策略
- Identify high-maintenance printers and establish proactive maintenance strategies
- - 找出影響設備壽命的關鍵因素
- Determine key factors that reduce equipment lifespan
- - 使用機器學習（隨機森林）預測錯誤發生機率
- Predict error probability using machine learning (Random Forest)
- - 透過排程優化提出降低成本的建議
- Recommend cost-saving measures through optimized scheduling
- - 利用資料洞察最小化非計劃停機時間
- Minimize unplanned downtime using data-driven insights

---

## File Structure | 檔案結構

| 檔案 / File | 說明 / Description |
|---|---|
| `README.md` | 專案描述 / Description |
| `EDA_copy.ipynb` | 初步探索性資料分析，包含各印表機的過濾流量時間序列圖與錯誤時間點標記 / Initial EDA with filter flow time-series and error marker visualizations per printer |
| `Model prep_copy.ipynb` | 模型前置處理：標記 `is_error`、特徵工程、時間窗口對齊 / Model preprocessing: error labeling (±7hr window), feature engineering, time alignment |
| `InkSight_copy.ipynb` | 主要預測模型（Random Forest），訓練並評估錯誤預測機率，支援新資料輸入 / Main prediction model using Random Forest; trains on historical data and predicts error probability for new input |

---

## Analysis Summary | 分析摘要

### 1. Exploratory Data Analysis (EDA) | 探索性分析
- 分析 30 台印表機（Printer 1–30）的 filter flow 時間序列
- 標記各時間點的錯誤發生位置
- 計算 Pearson 與 Spearman 相關係數，評估流量變化與錯誤順序之間的關聯

### 2. Data Collection Pattern Analysis | 資料收集模式分析
- 視覺化各印表機、各墨水顏色（C/M/Y/K/CG）的資料收集週期與間隔分佈
- 分析每月資料量，找出異常稀疏或密集的時間段

### 3. Machine Learning — Error Prediction | 機器學習錯誤預測
- 模型：Random Forest Classifier（100 棵決策樹）
- 標記方式：錯誤發生前後 ±7 小時內的過濾流量記錄標記為 `is_error = 1`
- 特徵包含：`filter_flow`、`weekday`、`hour`、`avg_flow`、`max_flow`、`min_flow`、`day_night`
- 評估指標：Classification Report、ROC AUC Score

### 4. Cross-Printer Comparison | 跨印表機比較
- K-Means 分群將印表機依特徵（平均流量、資料收集頻率、錯誤率）分組
- 熱圖比較各印表機各顏色的錯誤分佈
- 分析過濾流量穩定性與錯誤發生的關聯

---

## Tech Stack | 使用技術

| 類別 | 工具 |
|---|---|
| 語言 | Python 3.9 |
| 資料處理 | pandas, numpy |
| 視覺化 | matplotlib, seaborn |
| 機器學習 | scikit-learn (RandomForestClassifier, KMeans) |
| 統計分析 | scipy (Pearson, Spearman correlation) |
| 資料格式 | CSV |
| 開發環境 | Jupyter Notebook, VS Code |

---

## How to Run | 執行方式

```bash
# 1. Clone the repository
git clone https://github.com/Morriswuu/PRJ4-2025-D02-Canon-Project.git

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn scipy pyarrow

# 3. Place data files in the project root directory
# Required: filterflow_data.csv
#           error_data_clean.csv

# 4. Open notebooks in order
# EDA_copy.ipynb → printer_analyze.ipynb → Model prep_copy.ipynb → InkSight_copy.ipynb
```
---

## Key Findings | 主要發現

- 過濾流量的異常讀值（z-score ≥ 2）在後續 24 小時內發生錯誤的機率顯著高於正常讀值
- 不同印表機與墨水顏色之間存在明顯的錯誤分佈差異
- 特定時段（星期與小時）的錯誤頻率較高，可作為預防性維護的排程依據

---

## Author | 作者

**Morris Wu, Kevin**
Fontys University of Applied Sciences — ICT & Data Science
Project: PRJ4-2025-D02 | Group Project

---

## License | 授權

This project is for academic purposes only. Data and results are confidential.
本專案僅供學術用途，資料與結果屬機密性質。
