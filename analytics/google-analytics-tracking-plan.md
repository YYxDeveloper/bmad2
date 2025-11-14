# 鄰里市集 - Google Analytics 4 追蹤計劃

**目的**: 定義App內關鍵事件與轉換目標，追蹤用戶行為與產品成效
**分析工具**: Google Analytics 4 (GA4) + Firebase Analytics
**更新日期**: 2025-11-13
**版本**: 1.0

---

## 目錄

1. [關鍵指標定義（North Star Metrics）](#關鍵指標定義)
2. [事件架構設計](#事件架構設計)
3. [核心事件追蹤](#核心事件追蹤)
4. [轉換目標設定](#轉換目標設定)
5. [用戶旅程追蹤](#用戶旅程追蹤)
6. [漏斗分析設定](#漏斗分析設定)
7. [自定義維度與指標](#自定義維度與指標)
8. [儀表板設計](#儀表板設計)
9. [實作代碼範例](#實作代碼範例)
10. [數據治理規範](#數據治理規範)

---

## 關鍵指標定義（North Star Metrics）

### 北極星指標

**主要指標**: **月活躍交易用戶數（Monthly Active Transaction Users, MATU）**

**定義**: 每月至少完成一次交易（買或賣）的獨立用戶數

**為什麼選這個指標？**
- 代表平台真實價值（不只註冊，而是實際使用）
- 結合了供給端（賣家）與需求端（買家）
- 與商業目標直接相關（未來抽成基於交易量）
- 可分解為多個子指標進行優化

---

### 關鍵輔助指標（KPIs）

#### 1. 用戶獲取指標

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **新用戶數** | 每月新註冊用戶 | 100+/月 | 每日 |
| **下載轉註冊率** | 下載App後完成註冊的比例 | > 60% | 每週 |
| **註冊轉驗證率** | 註冊後完成社區驗證的比例 | > 40% | 每週 |
| **驗證轉發布率** | 驗證後發布首件商品的比例 | > 30% | 每週 |
| **CAC（用戶獲取成本）** | 每獲取一個驗證用戶的成本 | < NT$50 | 每月 |

#### 2. 用戶活躍度指標

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **DAU（日活躍用戶）** | 每日打開App的獨立用戶數 | 20+ | 每日 |
| **MAU（月活躍用戶）** | 每月打開App的獨立用戶數 | 200+ | 每月 |
| **DAU/MAU比率** | 用戶黏性指標 | > 15% | 每週 |
| **平均使用時長** | 每次session平均時長 | > 3分鐘 | 每週 |
| **每週使用頻率** | 每週平均打開App次數 | > 3次 | 每週 |

#### 3. 供給端指標（賣家）

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **商品發布數** | 每月新發布的商品數量 | 100+件/月 | 每日 |
| **活躍賣家數** | 每月至少發布1件商品的用戶數 | 30+/月 | 每週 |
| **平均商品上架時間** | 從發布到售出/下架的平均天數 | < 7天 | 每週 |
| **商品售出率** | 商品最終售出的比例 | > 25% | 每月 |
| **賣家留存率** | 發布後持續活躍的賣家比例 | > 40% | 每月 |

#### 4. 需求端指標（買家）

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **商品瀏覽數** | 每月商品詳情頁瀏覽次數 | 1000+/月 | 每日 |
| **留言數** | 每月商品留言數量 | 200+/月 | 每日 |
| **活躍買家數** | 每月至少留言1次的用戶數 | 50+/月 | 每週 |
| **瀏覽轉留言率** | 瀏覽商品後發表留言的比例 | > 20% | 每週 |
| **留言轉交易率** | 留言後完成交易的比例 | > 30% | 每週 |

#### 5. 交易指標

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **交易完成數** | 每月成功完成的交易數量 | 100+筆/月 | 每日 |
| **交易完成率** | 發布商品最終完成交易的比例 | > 25% | 每週 |
| **平均交易金額** | 每筆交易的平均金額 | NT$500-1000 | 每週 |
| **交易GMV** | 總交易金額（Gross Merchandise Value） | NT$50,000+/月 | 每月 |
| **評價完成率** | 交易後雙方完成評價的比例 | > 80% | 每週 |

#### 6. 留存指標

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **次日留存（D1）** | 註冊後第2天回來的用戶比例 | > 50% | 每日 |
| **週留存（D7）** | 註冊後第7天回來的用戶比例 | > 35% | 每週 |
| **月留存（D30）** | 註冊後第30天回來的用戶比例 | > 40% | 每月 |
| **交易留存** | 完成首次交易後持續交易的比例 | > 50% | 每月 |

#### 7. 社區健康度指標

| 指標 | 定義 | 目標值（MVP） | 追蹤頻率 |
|------|------|---------------|----------|
| **社區滲透率** | 已驗證用戶 / 社區總戶數 | > 10% | 每週 |
| **社區活躍度** | 每個社區每週新增商品數 | > 5件/週 | 每週 |
| **社區交易密度** | 每個社區每月交易數 | > 10筆/月 | 每月 |
| **跨社區交易比例** | 買賣雙方來自不同社區的交易比例 | < 5%（越低越好）| 每月 |

---

## 事件架構設計

### 事件命名規範

**格式**: `{object}_{action}` 或 `{action}_{object}`

**範例**:
- `item_view` - 查看商品
- `item_publish` - 發布商品
- `comment_post` - 發表留言
- `transaction_complete` - 完成交易

**規則**:
- 使用小寫字母
- 單詞間用底線分隔
- 動詞在前或在後保持一致
- 避免過長的事件名稱（< 40字元）

---

### 事件分類

#### Tier 1: 核心轉換事件（Critical）
**定義**: 直接影響北極星指標的關鍵行為
- `sign_up` - 註冊
- `community_verify` - 社區驗證
- `item_publish` - 發布商品
- `comment_post` - 發表留言
- `transaction_complete` - 完成交易
- `rating_submit` - 提交評價

#### Tier 2: 重要功能事件（Important）
**定義**: 重要但非核心轉換的功能使用
- `item_view` - 查看商品詳情
- `search` - 搜尋
- `filter_apply` - 套用篩選
- `negotiation_start` - 開始議價
- `pickup_method_select` - 選擇取貨方式
- `notification_click` - 點擊推播通知

#### Tier 3: 輔助分析事件（Nice-to-have）
**定義**: 用於深度分析與優化的輔助事件
- `onboarding_start` - 開始引導流程
- `onboarding_complete` - 完成引導
- `tutorial_view` - 觀看教學
- `photo_upload` - 上傳照片
- `url_paste` - 貼上商品網址
- `quick_reply_use` - 使用快速回覆

---

## 核心事件追蹤

### 1. 用戶註冊與驗證

#### Event: `sign_up`
**觸發時機**: 完成第三方登入註冊

**參數**:
```javascript
{
  method: string,           // 註冊方式: "google" | "apple" | "line"
  timestamp: number,        // 註冊時間戳
  referral_source: string   // 推薦來源: "organic" | "friend" | "ad" | "community_admin"
}
```

**轉換價值**: 高（註冊是第一步）

**追蹤目標**:
- 各第三方登入方式的轉換率
- 不同來源的註冊質量（後續留存率）

---

#### Event: `community_verify`
**觸發時機**: 完成社區身份驗證（掃描QR Code成功）

**參數**:
```javascript
{
  community_id: string,           // 社區ID
  community_name: string,         // 社區名稱
  verification_method: string,    // 驗證方式: "qr_code" | "manual"
  days_since_signup: number,      // 註冊後幾天才驗證
  verification_location: string   // 驗證地點: "management_office" | "other"
}
```

**轉換價值**: 極高（驗證後才能完整使用）

**追蹤目標**:
- 註冊到驗證的時間差（越短越好）
- 驗證轉換率
- 哪些社區驗證率較高

---

### 2. 商品發布

#### Event: `item_publish_start`
**觸發時機**: 點擊「發布商品」按鈕

**參數**:
```javascript
{
  entry_point: string  // 入口: "home_fab" | "profile_tab" | "onboarding_prompt"
}
```

---

#### Event: `item_publish_complete`
**觸發時機**: 成功發布商品

**參數**:
```javascript
{
  item_id: string,              // 商品ID
  category: string,             // 分類: "furniture" | "appliances" | "baby" | "books" | "clothing" | "sports" | "other"
  condition: string,            // 狀態: "new" | "used"
  price: number,                // 價格（NT$）
  is_negotiable: boolean,       // 是否可議價
  has_url: boolean,             // 是否有商品網址
  url_source: string,           // 網址來源: "momo" | "shopee" | "pchome" | "yahoo" | "other" | null
  usage_time: string,           // 使用時間: "unused" | "less_1m" | "1_6m" | "6m_1y" | "1_3y" | "3y_plus"
  photo_count: number,          // 照片數量（固定4）
  description_length: number,   // 摘要字數
  time_to_complete: number,     // 發布流程耗時（秒）
  user_type: string             // 用戶類型: "first_time" | "returning"
}
```

**轉換價值**: 極高（供給端核心行為）

**追蹤目標**:
- 各分類商品數量分布
- 新手 vs. 老手的發布時間差異
- 有網址 vs. 無網址的售出率差異
- 發布流程的平均耗時（目標< 2分鐘）

---

#### Event: `item_publish_abandoned`
**觸發時機**: 開始發布但中途放棄（離開頁面）

**參數**:
```javascript
{
  last_step: string,      // 最後完成的步驟: "photo" | "condition" | "url" | "title" | "description" | "usage" | "price"
  time_spent: number,     // 花了多少時間（秒）
  abandon_reason: string  // 放棄原因（若有彈窗詢問）
}
```

**追蹤目標**:
- 找出發布流程的摩擦點
- 優化流失率高的步驟

---

### 3. 商品瀏覽與搜尋

#### Event: `item_view`
**觸發時機**: 打開商品詳情頁

**參數**:
```javascript
{
  item_id: string,
  category: string,
  price: number,
  seller_id: string,
  seller_rating: number,        // 賣家評分
  seller_transaction_count: number, // 賣家交易次數
  days_since_published: number, // 發布後幾天
  view_source: string,          // 來源: "home_feed" | "search" | "category" | "notification" | "profile"
  is_same_community: boolean    // 是否同社區
}
```

**追蹤目標**:
- 哪些商品/分類最受歡迎
- 不同來源的轉換率差異
- 同社區 vs. 跨社區的瀏覽行為

---

#### Event: `search`
**觸發時機**: 執行搜尋

**參數**:
```javascript
{
  search_term: string,      // 搜尋關鍵字
  result_count: number,     // 結果數量
  filters_applied: array,   // 套用的篩選: ["price_low_high", "community_filter", "category_filter"]
  is_zero_results: boolean  // 是否零結果
}
```

**追蹤目標**:
- 熱門搜尋關鍵字（優化分類與標籤）
- 零結果搜尋（改進搜尋演算法）

---

### 4. 留言與議價

#### Event: `comment_post`
**觸發時機**: 發表留言

**參數**:
```javascript
{
  item_id: string,
  comment_type: string,       // 留言類型: "public" | "private"
  is_quick_reply: boolean,    // 是否使用快速回覆
  comment_length: number,     // 留言字數
  is_negotiation: boolean,    // 是否為議價留言
  user_role: string           // 角色: "buyer" | "seller" | "other"
}
```

**轉換價值**: 高（購買意向強）

---

#### Event: `negotiation_start`
**觸發時機**: 使用一鍵議價功能

**參數**:
```javascript
{
  item_id: string,
  original_price: number,     // 原價
  offered_price: number,      // 出價
  discount_percent: number,   // 折扣百分比
  negotiation_method: string  // 議價方式: "slider" | "input"
}
```

---

#### Event: `negotiation_respond`
**觸發時機**: 賣家回應議價

**參數**:
```javascript
{
  item_id: string,
  response_type: string,      // 回應類型: "accept" | "reject" | "counter_offer"
  counter_price: number,      // 反議價（若有）
  response_time: number       // 回應耗時（分鐘）
}
```

**追蹤目標**:
- 議價成功率
- 議價對成交的影響
- 賣家回應時間（越快越好）

---

### 5. 交易完成

#### Event: `pickup_method_select`
**觸發時機**: 選擇取貨方式

**參數**:
```javascript
{
  item_id: string,
  method: string,             // 取貨方式: "management_pickup" | "public_area" | "door_meetup"
  is_cash_only: boolean       // 是否現金交易（MVP階段都是true）
}
```

**追蹤目標**:
- 各取貨方式的使用率
- 管理室代收的使用率（驗證需求）

---

#### Event: `transaction_complete`
**觸發時機**: 交易標記為完成（買賣雙方確認）

**參數**:
```javascript
{
  transaction_id: string,
  item_id: string,
  seller_id: string,
  buyer_id: string,
  final_price: number,              // 最終成交價
  original_price: number,           // 原始定價
  discount_amount: number,          // 折扣金額
  pickup_method: string,            // 取貨方式
  payment_method: string,           // 付款方式: "cash" (MVP僅此)
  days_to_complete: number,         // 從發布到成交的天數
  comment_count: number,            // 該商品的留言數
  is_same_community: boolean,       // 是否同社區交易
  community_id: string              // 交易社區ID
}
```

**轉換價值**: 極高（核心轉換）

**追蹤目標**:
- 交易完成率
- 成交價 vs. 原價的折扣率
- 各取貨方式的成交率差異
- 從發布到成交的時間（越短越好）

---

### 6. 評價系統

#### Event: `rating_submit`
**觸發時機**: 提交評價

**參數**:
```javascript
{
  transaction_id: string,
  rating_score: number,       // 評分: 1-5
  has_comment: boolean,       // 是否有文字評價
  comment_length: number,     // 評價字數
  user_role: string,          // 評價者角色: "buyer" | "seller"
  days_after_transaction: number // 交易後幾天評價
}
```

**追蹤目標**:
- 評價完成率（目標> 80%）
- 平均評分分布
- 負評比例與原因

---

### 7. 管理室代收專用事件

#### Event: `management_pickup_create`
**觸發時機**: 創建管理室代收訂單

**參數**:
```javascript
{
  item_id: string,
  pickup_code: string,        // 6位數代收編號
  estimated_amount: number    // 預估金額
}
```

---

#### Event: `management_pickup_delivered`
**觸發時機**: 賣家將商品送達管理室（管理員登記）

**參數**:
```javascript
{
  item_id: string,
  pickup_code: string,
  admin_id: string,           // 管理員ID
  delivery_time: number       // 從創建到送達的時間（小時）
}
```

---

#### Event: `management_pickup_collected`
**觸發時機**: 買家取貨（管理員登記）

**參數**:
```javascript
{
  item_id: string,
  pickup_code: string,
  admin_id: string,
  actual_amount: number,      // 實際金額
  collection_time: number     // 從送達到取貨的時間（小時）
}
```

**追蹤目標**:
- 管理室代收流程各階段耗時
- 管理室代收的完成率
- 哪些社區使用率較高

---

### 8. 推播通知

#### Event: `notification_receive`
**觸發時機**: 收到推播通知（自動記錄）

**參數**:
```javascript
{
  notification_type: string,  // 通知類型: "new_comment" | "comment_reply" | "item_sold" | "pickup_ready"
  item_id: string,
  is_foreground: boolean      // 是否前景收到
}
```

---

#### Event: `notification_click`
**觸發時機**: 點擊推播通知

**參數**:
```javascript
{
  notification_type: string,
  item_id: string,
  time_to_click: number       // 收到後幾分鐘點擊
}
```

**追蹤目標**:
- 推播通知點擊率
- 哪種類型通知最有效
- 推播到點擊的時間差

---

## 轉換目標設定

### GA4 轉換事件配置

在GA4中將以下事件標記為「轉換」：

| 轉換事件 | 優先級 | 目標值（MVP） | 備註 |
|---------|--------|---------------|------|
| `sign_up` | P1 | 100+/月 | 新用戶註冊 |
| `community_verify` | P0 | 40+/月 | 驗證是啟動關鍵 |
| `item_publish_complete` | P0 | 100+/月 | 供給端核心 |
| `comment_post` | P1 | 200+/月 | 購買意向 |
| `transaction_complete` | P0 | 100+/月 | 最終轉換 |
| `rating_submit` | P1 | 160+/月 | 交易質量指標 |

---

### 轉換漏斗（Conversion Funnels）

#### 漏斗1：新用戶啟動（User Activation）

```
下載App (100%)
   ↓ 目標> 60%
註冊成功 (60%)
   ↓ 目標> 40%
社區驗證 (24%)
   ↓ 目標> 30%
發布首件商品 (7.2%)
```

**優化重點**:
- 提升註冊率：優化Onboarding體驗
- 提升驗證率：降低驗證門檻、提供試用期
- 提升首次發布率：引導教學、簡化流程

---

#### 漏斗2：商品交易（Item Transaction）

```
商品發布 (100%)
   ↓ 目標> 80%
收到首條留言 (80%)
   ↓ 目標> 50%
開始議價/約定 (40%)
   ↓ 目標> 75%
交易完成 (30%)
   ↓ 目標> 80%
雙方評價 (24%)
```

**優化重點**:
- 提升留言率：優化商品展示、推薦機制
- 提升交易完成率：簡化交易流程、提供管理室代收
- 提升評價率：推播提醒、簡化評價流程

---

#### 漏斗3：買家購買（Buyer Purchase）

```
首頁瀏覽 (100%)
   ↓ 目標> 30%
點擊商品 (30%)
   ↓ 目標> 20%
發表留言 (6%)
   ↓ 目標> 30%
完成交易 (1.8%)
```

**優化重點**:
- 提升點擊率：優化商品卡設計、照片品質
- 提升留言率：降低留言門檻、快速回覆範本
- 提升成交率：一鍵議價、管理室代收

---

## 用戶旅程追蹤

### 關鍵用戶旅程事件序列

#### 旅程1：首次賣家（王美玲 - 退休阿姨）

```javascript
// 第1天
sign_up { method: "line" }
  → onboarding_start
  → onboarding_view { step: "community_verify_prompt" }
  → community_verify { days_since_signup: 0 }

// 第2天
item_publish_start { entry_point: "onboarding_prompt" }
  → photo_upload { count: 4 }
  → url_paste { source: "momo" }
  → item_publish_complete { time_to_complete: 180, user_type: "first_time" }

// 第3天
notification_receive { type: "new_comment" }
  → notification_click
  → comment_view
  → comment_reply { is_quick_reply: true }

// 第5天
pickup_method_select { method: "management_pickup" }
  → management_pickup_create
  → management_pickup_delivered

// 第6天
management_pickup_collected
  → transaction_complete
  → rating_submit { rating_score: 5 }
```

**追蹤目標**:
- 從註冊到首次交易的天數（D1-D7）
- 管理室代收的使用率
- 快速回覆的使用率

---

#### 旅程2：新手媽媽（林雅婷）

```javascript
// 第1天
sign_up { method: "google" }
  → community_verify { days_since_signup: 0 }
  → search { search_term: "嬰兒推車" }
  → item_view { view_source: "search" }
  → comment_post { comment_type: "public" }

// 第2天
negotiation_start { discount_percent: 12.5 }
  → negotiation_respond { response_type: "accept" }
  → pickup_method_select { method: "door_meetup" }
  → transaction_complete
  → rating_submit

// 第10天（賣家行為）
item_publish_complete { category: "baby", has_url: true }
  → comment_receive
  → negotiation_respond { response_type: "accept" }
  → transaction_complete
```

**追蹤目標**:
- 搜尋到購買的轉換率
- 議價成功率
- 買賣雙向用戶的價值（更高LTV）

---

## 漏斗分析設定

### GA4 Funnel Exploration 配置

#### 漏斗1：用戶啟動漏斗

```
步驟1: app_open (首次打開)
步驟2: sign_up (註冊)
步驟3: community_verify (驗證)
步驟4: item_publish_complete OR comment_post (啟動)
```

**分析維度**:
- 按註冊方式（Google/Apple/LINE）
- 按推薦來源（organic/friend/ad）
- 按社區（community_id）

---

#### 漏斗2：賣家發布漏斗

```
步驟1: item_publish_start
步驟2: photo_upload (上傳照片)
步驟3: title_complete (完成標題)
步驟4: price_set (設定價格)
步驟5: item_publish_complete
```

**分析維度**:
- 按用戶類型（first_time/returning）
- 按分類（category）
- 按是否有網址（has_url）

---

#### 漏斗3：買家購買漏斗

```
步驟1: item_view
步驟2: comment_post OR negotiation_start
步驟3: pickup_method_select
步驟4: transaction_complete
步驟5: rating_submit
```

**分析維度**:
- 按商品分類（category）
- 按價格區間（<500, 500-1000, 1000-2000, >2000）
- 按取貨方式（pickup_method）

---

## 自定義維度與指標

### 用戶屬性（User Properties）

在GA4中設定以下用戶屬性：

| 屬性名稱 | 類型 | 範例值 | 說明 |
|---------|------|--------|------|
| `user_community` | string | "community_001" | 用戶所屬社區 |
| `user_verified` | boolean | true | 是否已驗證 |
| `user_signup_method` | string | "google" | 註冊方式 |
| `user_role` | string | "buyer_seller" | 用戶角色（buyer/seller/both） |
| `user_transaction_count` | number | 5 | 交易次數 |
| `user_rating` | number | 4.8 | 用戶評分 |
| `user_days_since_signup` | number | 15 | 註冊天數 |
| `user_cohort` | string | "2025-11-W1" | 註冊週期（Cohort） |

---

### 自定義指標（Custom Metrics）

| 指標名稱 | 計算方式 | 說明 |
|---------|---------|------|
| `avg_time_to_publish` | AVG(time_to_complete) | 平均發布耗時 |
| `avg_item_lifespan` | AVG(days_to_complete) | 商品平均上架天數 |
| `negotiation_success_rate` | negotiation_respond{accept} / negotiation_start | 議價成功率 |
| `transaction_value` | SUM(final_price) | 交易總額（GMV） |
| `avg_discount_rate` | AVG(discount_amount / original_price) | 平均折扣率 |

---

## 儀表板設計

### Dashboard 1: 北極星指標儀表板

**KPI卡片**:
- 本月MATU（目標100）
- 本月交易數（目標100筆）
- 本月GMV（目標NT$50,000）
- 本月新增驗證用戶（目標40）

**圖表**:
1. 每日MATU趨勢圖（折線圖）
2. 本月 vs. 上月對比（柱狀圖）
3. 各社區MATU分布（橫條圖）
4. 用戶角色分布（圓餅圖：純買家/純賣家/買賣雙方）

---

### Dashboard 2: 用戶獲取儀表板

**KPI卡片**:
- 本週新用戶數
- 註冊轉換率（下載→註冊）
- 驗證轉換率（註冊→驗證）
- CAC（用戶獲取成本）

**圖表**:
1. 用戶獲取漏斗（下載→註冊→驗證→啟動）
2. 各註冊方式轉換率對比（Google/Apple/LINE）
3. 各推薦來源質量對比（留存率）
4. 每日新用戶趨勢

---

### Dashboard 3: 交易健康度儀表板

**KPI卡片**:
- 本月商品發布數
- 本月交易完成數
- 商品售出率
- 評價完成率

**圖表**:
1. 交易漏斗（發布→留言→交易→評價）
2. 各分類商品數量與售出率（雙軸圖）
3. 各取貨方式使用率（圓餅圖）
4. 平均交易週期（發布到成交天數）

---

### Dashboard 4: 社區健康度儀表板

**維度**: 按社區拆分

**KPI卡片**（每社區）:
- 驗證用戶數
- 滲透率（驗證用戶/總戶數）
- 本週商品發布數
- 本月交易數

**圖表**:
1. 各社區滲透率排行（橫條圖）
2. 各社區活躍度熱力圖
3. 社區規模 vs. 交易密度（散點圖）

---

## 實作代碼範例

### Flutter + Firebase Analytics 整合

#### 1. 初始化 Firebase Analytics

```dart
// lib/services/analytics_service.dart
import 'package:firebase_analytics/firebase_analytics.dart';

class AnalyticsService {
  static final FirebaseAnalytics _analytics = FirebaseAnalytics.instance;
  static final FirebaseAnalyticsObserver observer =
      FirebaseAnalyticsObserver(analytics: _analytics);

  // 設定用戶屬性
  static Future<void> setUserProperties({
    required String userId,
    required String communityId,
    required bool isVerified,
    required String signupMethod,
  }) async {
    await _analytics.setUserId(id: userId);
    await _analytics.setUserProperty(
      name: 'user_community',
      value: communityId,
    );
    await _analytics.setUserProperty(
      name: 'user_verified',
      value: isVerified.toString(),
    );
    await _analytics.setUserProperty(
      name: 'user_signup_method',
      value: signupMethod,
    );
  }

  // 更新用戶角色
  static Future<void> updateUserRole(String role) async {
    await _analytics.setUserProperty(
      name: 'user_role',
      value: role,
    );
  }
}
```

---

#### 2. 追蹤註冊事件

```dart
// 完成註冊時
Future<void> trackSignUp(String method, String? referralSource) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'sign_up',
    parameters: {
      'method': method, // "google" | "apple" | "line"
      'timestamp': DateTime.now().millisecondsSinceEpoch,
      'referral_source': referralSource ?? 'organic',
    },
  );
}
```

---

#### 3. 追蹤社區驗證

```dart
Future<void> trackCommunityVerify({
  required String communityId,
  required String communityName,
  required int daysSinceSignup,
}) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'community_verify',
    parameters: {
      'community_id': communityId,
      'community_name': communityName,
      'verification_method': 'qr_code',
      'days_since_signup': daysSinceSignup,
      'verification_location': 'management_office',
    },
  );
}
```

---

#### 4. 追蹤商品發布

```dart
Future<void> trackItemPublishComplete({
  required String itemId,
  required String category,
  required String condition,
  required double price,
  required bool isNegotiable,
  required bool hasUrl,
  String? urlSource,
  required String usageTime,
  required int photoCount,
  required int descriptionLength,
  required int timeToComplete,
  required bool isFirstTime,
}) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'item_publish_complete',
    parameters: {
      'item_id': itemId,
      'category': category,
      'condition': condition,
      'price': price,
      'is_negotiable': isNegotiable,
      'has_url': hasUrl,
      'url_source': urlSource,
      'usage_time': usageTime,
      'photo_count': photoCount,
      'description_length': descriptionLength,
      'time_to_complete': timeToComplete,
      'user_type': isFirstTime ? 'first_time' : 'returning',
    },
  );
}
```

---

#### 5. 追蹤交易完成

```dart
Future<void> trackTransactionComplete({
  required String transactionId,
  required String itemId,
  required String sellerId,
  required String buyerId,
  required double finalPrice,
  required double originalPrice,
  required String pickupMethod,
  required int daysToComplete,
  required int commentCount,
  required bool isSameCommunity,
  required String communityId,
}) async {
  final discountAmount = originalPrice - finalPrice;

  await FirebaseAnalytics.instance.logEvent(
    name: 'transaction_complete',
    parameters: {
      'transaction_id': transactionId,
      'item_id': itemId,
      'seller_id': sellerId,
      'buyer_id': buyerId,
      'final_price': finalPrice,
      'original_price': originalPrice,
      'discount_amount': discountAmount,
      'pickup_method': pickupMethod,
      'payment_method': 'cash',
      'days_to_complete': daysToComplete,
      'comment_count': commentCount,
      'is_same_community': isSameCommunity,
      'community_id': communityId,
    },
  );

  // 同時追蹤為GA4 Revenue Event
  await FirebaseAnalytics.instance.logEvent(
    name: 'purchase',
    parameters: {
      'transaction_id': transactionId,
      'value': finalPrice,
      'currency': 'TWD',
      'items': [
        {
          'item_id': itemId,
          'item_name': 'Secondhand Item',
          'item_category': category,
          'price': finalPrice,
          'quantity': 1,
        }
      ],
    },
  );
}
```

---

#### 6. 追蹤議價事件

```dart
Future<void> trackNegotiationStart({
  required String itemId,
  required double originalPrice,
  required double offeredPrice,
}) async {
  final discountPercent = ((originalPrice - offeredPrice) / originalPrice * 100).round();

  await FirebaseAnalytics.instance.logEvent(
    name: 'negotiation_start',
    parameters: {
      'item_id': itemId,
      'original_price': originalPrice,
      'offered_price': offeredPrice,
      'discount_percent': discountPercent,
      'negotiation_method': 'slider',
    },
  );
}

Future<void> trackNegotiationRespond({
  required String itemId,
  required String responseType, // "accept" | "reject" | "counter_offer"
  double? counterPrice,
  required int responseTimeMinutes,
}) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'negotiation_respond',
    parameters: {
      'item_id': itemId,
      'response_type': responseType,
      'counter_price': counterPrice,
      'response_time': responseTimeMinutes,
    },
  );
}
```

---

#### 7. 追蹤管理室代收

```dart
Future<void> trackManagementPickupCreate({
  required String itemId,
  required String pickupCode,
  required double estimatedAmount,
}) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'management_pickup_create',
    parameters: {
      'item_id': itemId,
      'pickup_code': pickupCode,
      'estimated_amount': estimatedAmount,
    },
  );
}

Future<void> trackManagementPickupDelivered({
  required String itemId,
  required String pickupCode,
  required String adminId,
  required int deliveryTimeHours,
}) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'management_pickup_delivered',
    parameters: {
      'item_id': itemId,
      'pickup_code': pickupCode,
      'admin_id': adminId,
      'delivery_time': deliveryTimeHours,
    },
  );
}
```

---

#### 8. 追蹤推播通知

```dart
Future<void> trackNotificationClick({
  required String notificationType,
  required String itemId,
  required int timeToClickMinutes,
}) async {
  await FirebaseAnalytics.instance.logEvent(
    name: 'notification_click',
    parameters: {
      'notification_type': notificationType,
      'item_id': itemId,
      'time_to_click': timeToClickMinutes,
    },
  );
}
```

---

#### 9. 追蹤畫面瀏覽

```dart
// 在每個畫面的 initState() 中
@override
void initState() {
  super.initState();
  FirebaseAnalytics.instance.setCurrentScreen(
    screenName: 'ItemDetailScreen',
    screenClassOverride: 'ItemDetailScreen',
  );
}
```

---

## 數據治理規範

### 隱私合規

**遵守規範**:
- ✅ GDPR（歐盟一般資料保護規則）
- ✅ PDPA（台灣個人資料保護法）
- ✅ Apple App Store 隱私要求
- ✅ Google Play 隱私政策

**不追蹤的資料**:
- ❌ 真實姓名
- ❌ 電話號碼
- ❌ Email完整內容
- ❌ 門牌號碼
- ❌ 聊天內容全文

**匿名化處理**:
- user_id 使用 UUID（不包含個人識別資訊）
- community_id 使用加密ID（不暴露社區名稱）
- 商品描述僅追蹤「字數」不追蹤「內容」

---

### 資料保存期限

| 資料類型 | 保存期限 | 備註 |
|---------|---------|------|
| 用戶事件資料 | 14個月 | GA4預設保留期限 |
| 用戶屬性 | 用戶刪除帳號後30天 | 遵守被遺忘權 |
| 轉換資料 | 無限期 | 用於商業分析（匿名化） |
| 原始事件日誌 | 90天 | 用於除錯與異常偵測 |

---

### 數據品質檢查

**每週檢查清單**:
- [ ] 檢查事件發送量（是否有異常下降/上升）
- [ ] 檢查關鍵轉換率（是否在正常範圍）
- [ ] 檢查參數完整性（是否有missing values）
- [ ] 檢查異常值（例如：price = 0, time_to_complete = 0）

**監控告警**:
- 日活下降> 20% → 發送告警
- 交易完成數< 3筆/日 → 發送告警
- 事件發送失敗率> 5% → 發送告警

---

## 附錄：事件快速參考

### 核心事件清單（按優先級）

| 事件名稱 | 觸發時機 | 轉換 | Tier |
|---------|---------|------|------|
| `sign_up` | 完成註冊 | ✅ | 1 |
| `community_verify` | 完成驗證 | ✅ | 1 |
| `item_publish_complete` | 發布商品 | ✅ | 1 |
| `comment_post` | 發表留言 | ✅ | 1 |
| `transaction_complete` | 完成交易 | ✅ | 1 |
| `rating_submit` | 提交評價 | ✅ | 1 |
| `item_view` | 查看商品 | ❌ | 2 |
| `search` | 搜尋 | ❌ | 2 |
| `negotiation_start` | 開始議價 | ❌ | 2 |
| `pickup_method_select` | 選擇取貨方式 | ❌ | 2 |
| `notification_click` | 點擊通知 | ❌ | 2 |
| `item_publish_abandoned` | 放棄發布 | ❌ | 3 |
| `photo_upload` | 上傳照片 | ❌ | 3 |
| `quick_reply_use` | 使用快速回覆 | ❌ | 3 |

---

**Google Analytics 追蹤計劃文檔完成！** 📊

---

## 下一步行動

1. **開發前準備**:
   - 在Firebase Console建立專案
   - 在GA4建立Property
   - 配置iOS/Android App

2. **開發階段**:
   - 整合Firebase Analytics SDK
   - 實作核心事件追蹤（Tier 1）
   - 測試事件發送（使用Debug View）

3. **上線前**:
   - 驗證所有轉換事件正常運作
   - 設定GA4儀表板
   - 配置告警機制

4. **上線後**:
   - 每日監控關鍵指標
   - 每週review數據品質
   - 每月分析用戶行為並優化產品
