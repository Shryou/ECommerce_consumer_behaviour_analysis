＃Hunter’s e-grocery：需求預測與消費者洞察分析系統
📌 專案概述
本專案針對美國電商平台 Hunter’s e-grocery 進行深入的數據分析。在後疫情時代與全球供應鏈不穩定的背景下，透過機器學習模型分析超過 200 萬筆交易紀錄，旨在解決庫存波動、預測消費者需求，並針對健康導向商品（如無乳糖、植物基產品）提供個人化推薦。

🎯 核心目標
1. 業務目標 (Business Goals)
需求預測精準化：降低生鮮食品的缺貨率與過期損耗。

個人化行銷策略：識別高潛力健康產品群眾，提升客戶滿意度。

經濟洞察：將非必要商品的消費模式作為「經濟信心指標」。

2. 技術目標 (Machine Learning Goals)
客群細分 (Segmentation)：利用 K-Means 演算法根據購買行為對用戶進行分類。

推薦系統 (Recommendation)：建立協同過濾 (Collaborative Filtering) 模型以精準預測用戶潛在需求。

📊 資料集說明
本分析採用約 2,019,501 筆原始數據，包含 12 個特徵維度：

order_id / user_id: 訂單與用戶識別。

days_since_prior_order: 距離前一次下單天數（用於衡量回購週期）。

product_name / department: 商品名稱與所屬類別。

order_dow / order_hour_of_day: 下單的時間維度。

⚙️ 技術架構
1. 資料前處理
缺失值處理：針對首購用戶的 days_since_prior_order 填補為 -1，區分新老客戶。

特徵工程：彙整用戶購買頻率、客單價平均值及部門偏好比例。

2. 分析模型
K-Means 聚類分析：

識別出「乳製品偏好群」、「有機蔬果支持者」等核心客群。

協同過濾 (CF)：

解決「買了什麼的人也會買什麼」的關聯預測。

Baseline 排行榜：

針對新用戶（冷啟動問題）提供基於熱門度的推薦。

📈 關鍵洞察
時間效應：分析顯示特定週間與時段具有顯著的採購高峰，可用於優化物流人力配置。

精準推送：

Cluster 2 (Dairy Group)：應推送早餐組合或乳製品折扣。

Clusters 1, 4, 5 (Organic Group)：應加強有機蔬果與健康食品的廣告覆蓋。

🚀 未來展望
深度學習引入：計畫導入 Neural Collaborative Filtering (NCF) 捕捉更複雜的非線性關係。

多模態數據集成：整合社群媒體的情緒分析，以進一步提升預測準確率。

時序模型優化：利用 RNN/LSTM 處理具有時間序列特性的動態購買行為。

🛠️ 開發環境與工具
Language: Python 3.11

Libraries:

pandas, numpy: 資料處理

matplotlib, seaborn: 數據視覺化

scikit-learn: 機器學習建模

Environment: Jupyter Notebook / Kaggle Kernel

📂 檔案結構
Plaintext
.
├── big_data_final_da.ipynb   # 核心分析程式碼與模型實作
├── Project_Report.pdf        # 詳細書面研究報告 (英文版)
└── README.md                 # 專案說明文件
💡 如何在 README 中更吸睛？
加入圖表：建議從你的 ipynb 中截取 1-2 張最關鍵的圖（例如：各群體的雷達圖或訂單分布圖），放在「關鍵洞察」區塊。

強調背景：你在自我介紹中提到是「資訊與財金管理」背景，這份 README 展現了你如何結合財金洞察（經濟信心指標）與技術實作，這對面試官非常有吸引力。
