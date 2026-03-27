# ECommerce Consumer Analysis

以電商訂單資料為基礎，完成從探索分析到推薦與分群的端到端分析專案。  
專案內容包含：

- EDA（探索式資料分析）
- Apriori 關聯規則分析（Association Rule Mining）
- 協同過濾推薦系統（Collaborative Filtering）
- K-Means 使用者分群（Customer Segmentation）

## 專案目標

1. 了解消費者的下單行為（週期、時段、訂單規模）
2. 找出商品共購關係，支援交叉銷售策略
3. 建立推薦模型並比較不同演算法成效
4. 將使用者分群，支援分眾行銷與營運決策

## 資料來源

- Dataset: `ECommerce_consumer behaviour.csv`
- 原始路徑（Notebook 版本）：
  - `/kaggle/input/ecommerce-dataset-for-predictive-marketing-2023/ECommerce_consumer behaviour.csv`

### 資料規模（Notebook 執行結果）

- 商品列（item-level）: `2,019,501` rows
- 訂單層級（order-level）: `200,000` orders
- 每筆訂單平均商品數: `10.10`
- 協同過濾評估使用者數: `82,352`

## 分析流程

### 1) EDA 與資料前處理

- 缺失值處理：`days_since_prior_order` 以 `-1` 表示首次購買
- 建立訂單層級資料，避免同一張訂單因多商品重複加權
- 特徵工程：
  - `total_items_per_order`
  - `average_days_between_orders`

重點觀察（Notebook 輸出）：

- 訂單大小：平均 `10.10`、中位數 `8`、最大 `137`
- 訂單間隔（排除首次購買）：
  - 平均 `11.00` 天
  - 中位數 `7` 天
  - `7` 天週期訂單數：`18,882`
  - `30` 天週期訂單數：`20,982`

### 2) Apriori 關聯規則

- `min_support=0.05`
- `metric='confidence'`
- `min_threshold=0.8`

範例規則（節錄）：

- `(fresh herbs, fresh fruits) -> (fresh vegetables)`
  - confidence: `0.881`
  - lift: `1.983`
- `(fresh vegetables, packaged vegetables fruits, yogurt) -> (fresh fruits)`
  - confidence: `0.866`
  - lift: `1.558`

### 3) 協同過濾推薦系統（5-Fold）

比較模型：

- Item-Based CF (`KNNBasic`, cosine, `k=30`)
- SVD
- BaselineOnly
- Random baseline (`NormalPredictor`)

評估指標：`Precision@10`, `Recall@10`, `NDCG@10`, `HitRate@10`, `AUC(sampled)`

平均結果（5 folds）：

| Model | Precision@10 | Recall@10 | NDCG@10 | HitRate@10 | AUC(sampled) |
|---|---:|---:|---:|---:|---:|
| BaselineOnly | 0.1093 | 0.4390 | 0.4868 | 0.7077 | 0.8227 |
| SVD | 0.1090 | 0.4377 | 0.4861 | 0.7070 | 0.8206 |
| Item-Based CF (KNN k=30) | 0.0954 | 0.3105 | 0.2862 | 0.5324 | 0.8209 |
| Random (NormalPredictor) | 0.0211 | 0.0804 | 0.0938 | 0.1917 | 0.5000 |

### 4) K-Means 使用者分群

方法重點：

- 行為特徵 + 部門偏好特徵
- log1p 處理極端值（購買量相關欄位）
- `StandardScaler` 標準化行為特徵
- 部門占比特徵使用 `PCA(n_components=0.9)` 壓縮
- 掃描 `k=2~20`，以多指標評估（Silhouette / CH / DB / Stability ARI）

候選群數（自動挑選結果）：`[2, 3, 4, 5, 19, 20]`

## 視覺化輸出（Notebook 內）

- 訂單大小分布（boxplot）
- 週內下單分布（bar / polar）
- 下單時段分布（hour-of-day）
- 訂單間隔直方圖
- KMeans 指標曲線（Elbow / Silhouette / CH / DB / Stability）
- 各群輪廓圖（群大小、行為熱圖、PCA 2D 投影、部門占比 grouped bar）

## 專案結構

```text
.
├── big_data_final_da (1).ipynb
└── README.md
```

## 執行方式

### 在 Kaggle 執行（建議）

1. 上傳 notebook
2. 掛載對應 dataset
3. 直接執行所有 cells

### 在本機執行

1. 準備資料檔 `ECommerce_consumer behaviour.csv`
2. 安裝依賴：
   - `numpy`, `pandas`, `matplotlib`, `seaborn`
   - `scikit-learn`, `mlxtend`, `scikit-surprise`, `tqdm`
3. 將 notebook 中資料路徑改為本機路徑後執行

## Business Value

- 關聯規則可用於「你可能也會買」與組合促銷
- 推薦模型可用於首頁個人化排序與再行銷
- 使用者分群可支援分眾優惠、CRM 與留存策略

## License

僅供學術與作品集展示用途，若需商業使用請確認原始資料授權條款。
