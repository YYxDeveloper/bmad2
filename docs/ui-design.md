# 鄰里市集 - UI 設計規範

**Product:** 鄰里市集 (Neighbor Marketplace)
**Platform:** Flutter (iOS & Android)
**Designer:** UI Designer
**Date:** 2025-11-13
**Version:** 1.0

---

## 執行摘要

本文件定義鄰里市集的完整 UI 設計系統，包含設計系統基礎、組件庫、視覺規範、互動設計和無障礙設計。設計目標是創造一個**友善、信任、在地化**的視覺體驗，讓台灣社區住戶能夠輕鬆、安心地進行二手交易。

### 設計原則

1. **友善親切** - 溫暖的色調、圓潤的設計，讓使用者感到親近
2. **信任可靠** - 清晰的資訊架構、明確的狀態反饋，建立信任感
3. **在地化** - 符合台灣使用者的視覺習慣和文化認知
4. **易用性** - 考慮中高齡使用者，大字體、高對比、簡潔操作
5. **一致性** - 統一的設計語言，降低學習成本

---

## 1. 設計系統基礎

### 1.1 色彩系統

#### 主色調（Primary Colors）

**品牌主色：鄰里藍（Neighbor Blue）**
- `Primary-500`: `#2563EB` - 主要按鈕、連結、重要資訊
- `Primary-600`: `#1D4ED8` - 按鈕 hover 狀態
- `Primary-700`: `#1E40AF` - 按鈕 pressed 狀態
- `Primary-50`: `#EFF6FF` - 背景、選中狀態
- `Primary-100`: `#DBEAFE` - 徽章、標籤背景

**語義：** 藍色傳達信任、可靠、專業，符合社區交易平台的定位。

#### 輔助色調（Secondary Colors）

**成功綠（Success Green）**
- `Success-500`: `#10B981` - 成功狀態、完成標記
- `Success-50`: `#ECFDF5` - 成功背景

**警告橙（Warning Orange）**
- `Warning-500`: `#F59E0B` - 警告訊息、提醒
- `Warning-50`: `#FFFBEB` - 警告背景

**錯誤紅（Error Red）**
- `Error-500`: `#EF4444` - 錯誤訊息、危險操作
- `Error-50`: `#FEF2F2` - 錯誤背景

**資訊藍（Info Blue）**
- `Info-500`: `#3B82F6` - 資訊提示
- `Info-50`: `#EFF6FF` - 資訊背景

#### 中性色調（Neutral Colors）

**文字顏色**
- `Text-Primary`: `#1E293B` - 主要文字（標題、重要內容）
- `Text-Secondary`: `#64748B` - 次要文字（描述、輔助資訊）
- `Text-Tertiary`: `#94A3B8` - 第三級文字（提示、佔位符）
- `Text-Disabled`: `#CBD5E1` - 禁用狀態文字

**背景顏色**
- `Background-Primary`: `#FFFFFF` - 主要背景（頁面背景）
- `Background-Secondary`: `#F8F9FA` - 次要背景（卡片、區塊）
- `Background-Tertiary`: `#F1F5F9` - 第三級背景（輸入框、分隔線）
- `Background-Overlay`: `#1E293B80` - 遮罩層（60% 透明度）

**邊框顏色**
- `Border-Light`: `#E5E7EB` - 輕邊框（卡片、分隔線）
- `Border-Medium`: `#CBD5E1` - 中等邊框（輸入框）
- `Border-Dark`: `#94A3B8` - 深邊框（選中狀態）

#### 特殊顏色

**社區認證徽章**
- `Badge-Verified`: `#2563EB` - 認證徽章背景
- `Badge-Verified-Text`: `#FFFFFF` - 認證徽章文字

**商品狀態標籤**
- `Status-New`: `#10B981` - 全新商品
- `Status-Used`: `#F59E0B` - 二手商品
- `Status-Sold`: `#94A3B8` - 已售出

#### 色彩使用規範

1. **主色調使用**
   - 主要操作按鈕：使用 `Primary-500`
   - 重要資訊強調：使用 `Primary-500`
   - 連結文字：使用 `Primary-500`，hover 時使用 `Primary-600`

2. **語義色使用**
   - 成功狀態：使用 `Success-500`（如交易完成、評價成功）
   - 警告訊息：使用 `Warning-500`（如安全提示、注意事項）
   - 錯誤訊息：使用 `Error-500`（如驗證失敗、操作錯誤）
   - 資訊提示：使用 `Info-500`（如通知、說明）

3. **中性色使用**
   - 文字層級：嚴格遵循三級文字顏色系統
   - 背景層級：使用不同層級的背景色建立視覺層次
   - 邊框使用：根據重要性選擇邊框顏色

---

### 1.2 字體系統

#### 字體家族

**主要字體：**
- **中文：** `PingFang TC` (iOS) / `Noto Sans TC` (Android)
- **英文/數字：** `SF Pro Display` (iOS) / `Roboto` (Android)
- **備用字體：** `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Microsoft JhengHei', sans-serif`

**選擇理由：**
- PingFang TC 是 iOS 系統預設中文字體，清晰易讀
- Noto Sans TC 是 Google 開源字體，Android 系統支援良好
- 兩者都針對繁體中文優化，適合台灣使用者

#### 字體大小（Font Sizes）

**標題字體（Headings）**
- `H1`: `32px` / `2rem` - 頁面主標題（粗體 700）
- `H2`: `24px` / `1.5rem` - 區塊標題（粗體 600）
- `H3`: `20px` / `1.25rem` - 卡片標題（粗體 600）
- `H4`: `18px` / `1.125rem` - 小標題（粗體 600）

**正文字體（Body）**
- `Body-Large`: `18px` / `1.125rem` - 重要正文（一般 400）
- `Body-Medium`: `16px` / `1rem` - 標準正文（一般 400）
- `Body-Small`: `14px` / `0.875rem` - 次要正文（一般 400）
- `Body-XSmall`: `12px` / `0.75rem` - 輔助文字（一般 400）

**特殊字體**
- `Caption`: `12px` / `0.75rem` - 圖片說明、標籤文字（一般 400）
- `Label`: `14px` / `0.875rem` - 表單標籤（中等 500）
- `Button`: `16px` / `1rem` - 按鈕文字（中等 500）

#### 字體粗細（Font Weights）

- `Regular (400)`: 正文字體
- `Medium (500)`: 按鈕、標籤、強調文字
- `Semibold (600)`: 標題、重要資訊
- `Bold (700)`: 主標題、極重要資訊

#### 行高（Line Heights）

- `Tight`: `1.2` - 標題行高
- `Normal`: `1.5` - 正文行高
- `Relaxed`: `1.75` - 長段落行高

#### 字體使用規範

1. **標題層級**
   - 每個頁面只有一個 H1
   - H2 用於主要區塊標題
   - H3 用於卡片標題
   - H4 用於小節標題

2. **文字大小**
   - 最小字體大小：12px（符合無障礙標準）
   - 重要資訊：使用 16px 以上
   - 中高齡友善：關鍵資訊使用 18px 以上

3. **文字對比度**
   - 主要文字與背景對比度 ≥ 4.5:1（WCAG AA）
   - 重要文字與背景對比度 ≥ 7:1（WCAG AAA）

---

### 1.3 間距系統（Spacing System）

#### 基礎間距單位

使用 **8px 基礎網格系統**，所有間距都是 8 的倍數。

**間距標記（Spacing Tokens）**
- `Space-0`: `0px` - 無間距
- `Space-1`: `4px` - 極小間距（圖示與文字之間）
- `Space-2`: `8px` - 小間距（元素內部間距）
- `Space-3`: `12px` - 中小間距（相關元素之間）
- `Space-4`: `16px` - 標準間距（卡片內元素間距）
- `Space-5`: `20px` - 中等間距（區塊間距）
- `Space-6`: `24px` - 大間距（卡片之間）
- `Space-8`: `32px` - 超大間距（主要區塊之間）
- `Space-10`: `40px` - 極大間距（頁面區塊之間）
- `Space-12`: `48px` - 最大間距（頁面頂部/底部）

#### 間距使用規範

1. **卡片內部間距**
   - 卡片 padding: `16px` (Space-4)
   - 卡片內元素間距: `12px` (Space-3)

2. **卡片之間間距**
   - 列表卡片間距: `16px` (Space-4)
   - 網格卡片間距: `12px` (Space-3)

3. **頁面邊距**
   - 頁面左右邊距: `16px` (Space-4)
   - 頁面上下邊距: `20px` (Space-5)

4. **表單間距**
   - 表單標籤與輸入框: `8px` (Space-2)
   - 表單欄位之間: `20px` (Space-5)

---

### 1.4 圓角系統（Border Radius）

**圓角標記（Radius Tokens）**
- `Radius-None`: `0px` - 無圓角
- `Radius-Small`: `4px` - 小圓角（標籤、徽章）
- `Radius-Medium`: `8px` - 中等圓角（按鈕、輸入框、卡片）
- `Radius-Large`: `12px` - 大圓角（大卡片、模態框）
- `Radius-XLarge`: `16px` - 超大圓角（特殊卡片）
- `Radius-Full`: `9999px` - 完全圓角（頭像、圓形按鈕）

**圓角使用規範**
- 按鈕：使用 `Radius-Medium` (8px)
- 卡片：使用 `Radius-Medium` (8px) 或 `Radius-Large` (12px)
- 輸入框：使用 `Radius-Medium` (8px)
- 頭像：使用 `Radius-Full` (完全圓角)
- 標籤/徽章：使用 `Radius-Small` (4px)

---

### 1.5 陰影系統（Shadow System）

**陰影標記（Shadow Tokens）**

**小陰影（Small Shadow）**
```css
box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.05);
```
- 用途：卡片、按鈕 hover 狀態

**中等陰影（Medium Shadow）**
```css
box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
```
- 用途：浮動卡片、下拉選單

**大陰影（Large Shadow）**
```css
box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.1);
```
- 用途：模態框、彈出視窗

**超大陰影（XLarge Shadow）**
```css
box-shadow: 0px 20px 25px rgba(0, 0, 0, 0.15);
```
- 用途：重要模態框、全螢幕覆蓋

**陰影使用規範**
- 卡片：使用 `Small Shadow` 或 `Medium Shadow`
- 按鈕 hover：使用 `Small Shadow`
- 模態框：使用 `Large Shadow`
- 浮動按鈕：使用 `Medium Shadow`

---

### 1.6 動畫系統（Animation System）

#### 動畫時長（Duration）

- `Duration-Fast`: `150ms` - 快速動畫（hover、點擊反饋）
- `Duration-Normal`: `300ms` - 標準動畫（頁面轉場、狀態變化）
- `Duration-Slow`: `500ms` - 慢速動畫（複雜轉場、載入動畫）

#### 動畫曲線（Easing）

- `Ease-In`: `cubic-bezier(0.4, 0, 1, 1)` - 加速進入
- `Ease-Out`: `cubic-bezier(0, 0, 0.2, 1)` - 減速退出
- `Ease-InOut`: `cubic-bezier(0.4, 0, 0.2, 1)` - 加速進入、減速退出（標準）

#### 動畫類型

**過渡動畫（Transitions）**
- 顏色變化：`150ms ease-in-out`
- 大小變化：`300ms ease-in-out`
- 位置變化：`300ms ease-in-out`
- 透明度變化：`200ms ease-in-out`

**載入動畫（Loading Animations）**
- 骨架屏（Skeleton）：`1.5s ease-in-out infinite`
- 旋轉載入：`1s linear infinite`
- 脈衝動畫：`2s ease-in-out infinite`

**互動動畫（Interaction Animations）**
- 按鈕點擊：`100ms ease-out`（縮放 0.95）
- 卡片點擊：`200ms ease-out`（縮放 0.98）
- 滑動切換：`300ms ease-in-out`

---

## 2. 組件庫（Component Library）

### 2.1 按鈕（Buttons）

#### 主要按鈕（Primary Button）

**樣式規範：**
- 背景色：`Primary-500` (#2563EB)
- 文字顏色：`#FFFFFF`
- 字體大小：`16px`
- 字體粗細：`Medium (500)`
- 高度：`48px`
- 圓角：`8px`
- 內邊距：`16px 24px`
- 陰影：無（預設），hover 時使用 `Small Shadow`

**狀態：**
- **Default**: 背景 `Primary-500`，文字白色
- **Hover**: 背景 `Primary-600`，陰影 `Small Shadow`
- **Pressed**: 背景 `Primary-700`，縮放 `0.95`
- **Disabled**: 背景 `Background-Tertiary`，文字 `Text-Disabled`，不可點擊

**使用場景：**
- 主要操作（如「確認交易」「發布商品」）
- 表單提交
- 重要流程的下一步

#### 次要按鈕（Secondary Button）

**樣式規範：**
- 背景色：`#FFFFFF`
- 邊框：`2px solid Border-Medium` (#CBD5E1)
- 文字顏色：`Text-Primary` (#1E293B)
- 字體大小：`16px`
- 字體粗細：`Medium (500)`
- 高度：`48px`
- 圓角：`8px`
- 內邊距：`16px 24px`

**狀態：**
- **Default**: 白色背景，灰色邊框
- **Hover**: 背景 `Background-Secondary`，邊框 `Primary-500`
- **Pressed**: 背景 `Background-Tertiary`，縮放 `0.95`
- **Disabled**: 背景 `Background-Tertiary`，邊框 `Border-Light`，文字 `Text-Disabled`

**使用場景：**
- 次要操作（如「取消」「稍後」）
- 不需要強調的操作

#### 文字按鈕（Text Button）

**樣式規範：**
- 背景色：透明
- 文字顏色：`Primary-500`
- 字體大小：`16px`
- 字體粗細：`Medium (500)`
- 高度：`48px`
- 內邊距：`12px 16px`
- 無邊框、無背景

**狀態：**
- **Default**: 文字 `Primary-500`
- **Hover**: 背景 `Primary-50`，文字 `Primary-600`
- **Pressed**: 背景 `Primary-100`
- **Disabled**: 文字 `Text-Disabled`

**使用場景：**
- 連結式操作（如「查看詳情」「了解更多」）
- 不需要強調的次要操作

#### 圖示按鈕（Icon Button）

**樣式規範：**
- 尺寸：`48x48px`（觸控目標最小尺寸）
- 圓角：`8px`
- 背景：透明或 `Background-Secondary`
- 圖示大小：`24x24px`

**狀態：**
- **Default**: 背景透明
- **Hover**: 背景 `Background-Secondary`
- **Pressed**: 背景 `Background-Tertiary`，縮放 `0.9`

**使用場景：**
- 返回按鈕
- 更多選項按鈕
- 分享、收藏等操作

---

### 2.2 卡片（Cards）

#### 商品卡片（Product Card）

**樣式規範：**
- 背景色：`#FFFFFF`
- 圓角：`12px`
- 陰影：`Small Shadow`
- 內邊距：`16px`
- 間距：卡片之間 `16px`

**內容結構：**
```
┌─────────────────────────┐
│  [商品圖片]              │
│  (16:9 比例)             │
├─────────────────────────┤
│  商品標題 (H3)           │
│  價格 (Body-Large)       │
│  狀態標籤 (Badge)        │
│  賣家資訊 (Body-Small)   │
└─────────────────────────┘
```

**互動狀態：**
- **Default**: 正常顯示
- **Hover**: 陰影 `Medium Shadow`，輕微上移 `2px`
- **Pressed**: 縮放 `0.98`

#### 資訊卡片（Info Card）

**樣式規範：**
- 背景色：`Background-Secondary` (#F8F9FA)
- 圓角：`8px`
- 內邊距：`16px`
- 邊框：`1px solid Border-Light`（可選）

**使用場景：**
- 提示資訊
- 說明文字
- 狀態顯示

---

### 2.3 輸入框（Input Fields）

#### 標準輸入框（Text Input）

**樣式規範：**
- 高度：`48px`
- 背景色：`#FFFFFF`
- 邊框：`2px solid Border-Medium` (#CBD5E1)
- 圓角：`8px`
- 內邊距：`12px 16px`
- 字體大小：`16px`
- 文字顏色：`Text-Primary`

**狀態：**
- **Default**: 灰色邊框
- **Focus**: 邊框 `Primary-500`，背景 `Primary-50`
- **Error**: 邊框 `Error-500`，背景 `Error-50`
- **Disabled**: 背景 `Background-Tertiary`，邊框 `Border-Light`，文字 `Text-Disabled`

**標籤與提示：**
- 標籤：位於輸入框上方，`14px`，`Text-Secondary`
- 佔位符：`14px`，`Text-Tertiary`
- 錯誤訊息：位於輸入框下方，`12px`，`Error-500`

#### 多行輸入框（Textarea）

**樣式規範：**
- 最小高度：`120px`
- 背景色：`#FFFFFF`
- 邊框：`2px solid Border-Medium`
- 圓角：`8px`
- 內邊距：`12px 16px`
- 字體大小：`16px`
- 行高：`1.5`

**使用場景：**
- 商品描述
- 留言內容
- 長文字輸入

---

### 2.4 標籤與徽章（Badges & Tags）

#### 狀態標籤（Status Badge）

**全新商品標籤：**
- 背景色：`Success-50` (#ECFDF5)
- 文字顏色：`Success-500` (#10B981)
- 字體大小：`12px`
- 字體粗細：`Medium (500)`
- 圓角：`4px`
- 內邊距：`4px 8px`

**二手商品標籤：**
- 背景色：`Warning-50` (#FFFBEB)
- 文字顏色：`Warning-500` (#F59E0B)
- 其他樣式同全新標籤

**已售出標籤：**
- 背景色：`Background-Tertiary` (#F1F5F9)
- 文字顏色：`Text-Secondary` (#64748B)
- 其他樣式同全新標籤

#### 認證徽章（Verified Badge）

**樣式規範：**
- 背景色：`Badge-Verified` (#2563EB)
- 文字顏色：`#FFFFFF`
- 圖示：✓ 勾選圖示
- 字體大小：`12px`
- 圓角：`4px`
- 內邊距：`4px 8px`

**使用場景：**
- 已驗證社區成員
- 認證賣家
- 可信標記

---

### 2.5 頭像（Avatars）

#### 使用者頭像

**尺寸規格：**
- `Small`: `32x32px` - 列表中使用
- `Medium`: `48x48px` - 卡片中使用（標準）
- `Large`: `64x64px` - 個人主頁使用
- `XLarge`: `96x96px` - 個人主頁大頭像

**樣式規範：**
- 圓角：`Radius-Full` (完全圓角)
- 邊框：`2px solid Border-Light`（可選）
- 預設頭像：使用首字或預設圖示

**狀態：**
- **Default**: 正常顯示
- **Online**: 右下角顯示綠色圓點（`8x8px`）

---

### 2.6 圖片（Images）

#### 商品圖片

**尺寸規格：**
- 列表卡片：`16:9` 比例，寬度 `100%`
- 詳情頁：`1:1` 比例（正方形），最大寬度 `100%`
- 縮圖：`4:3` 比例

**樣式規範：**
- 圓角：`8px`（列表）或 `12px`（詳情）
- 載入狀態：顯示骨架屏（Skeleton）
- 錯誤狀態：顯示預設圖片

**互動：**
- 點擊放大：全螢幕查看
- 滑動切換：多張圖片時支援左右滑動

---

### 2.7 模態框（Modals）

#### 標準模態框

**樣式規範：**
- 背景：`Background-Overlay` (60% 透明度黑色)
- 內容區域：`#FFFFFF`，圓角 `16px`
- 最大寬度：`90%`（手機）或 `400px`（平板）
- 內邊距：`24px`
- 陰影：`Large Shadow`

**結構：**
```
┌─────────────────────────┐
│  標題 (H3)          [X]  │
├─────────────────────────┤
│  內容區域                │
│                         │
├─────────────────────────┤
│  [取消]        [確認]    │
└─────────────────────────┘
```

**互動：**
- 點擊背景：關閉模態框
- 點擊 X：關閉模態框
- 滑動手勢：向下滑動關閉（可選）

---

### 2.8 載入狀態（Loading States）

#### 骨架屏（Skeleton Screen）

**樣式規範：**
- 背景色：`Background-Tertiary` (#F1F5F9)
- 動畫：`1.5s ease-in-out infinite`（脈衝效果）
- 圓角：`4px`

**使用場景：**
- 商品列表載入
- 商品詳情載入
- 個人主頁載入

#### 旋轉載入（Spinner）

**樣式規範：**
- 尺寸：`40x40px`
- 顏色：`Primary-500`
- 動畫：`1s linear infinite`（旋轉）

**使用場景：**
- 按鈕載入狀態
- 頁面載入狀態
- 資料提交中

---

### 2.9 通知與提示（Notifications & Alerts）

#### 成功提示（Success Alert）

**樣式規範：**
- 背景色：`Success-50` (#ECFDF5)
- 邊框：`1px solid Success-500`
- 文字顏色：`Success-500`
- 圖示：✓ 勾選圖示
- 內邊距：`12px 16px`
- 圓角：`8px`

#### 錯誤提示（Error Alert）

**樣式規範：**
- 背景色：`Error-50` (#FEF2F2)
- 邊框：`1px solid Error-500`
- 文字顏色：`Error-500`
- 圖示：⚠️ 警告圖示
- 內邊距：`12px 16px`
- 圓角：`8px`

#### 推播通知（Push Notification）

**樣式規範：**
- 背景色：`#FFFFFF`
- 圓角：`12px`
- 陰影：`Large Shadow`
- 內邊距：`16px`
- 最大寬度：`90%`

**內容結構：**
```
┌─────────────────────────┐
│  [頭像] 標題              │
│        內容              │
│        時間              │
└─────────────────────────┘
```

---

## 3. 全域佈局系統（Global Layout System）

### 3.1 應用整體結構

**應用層級架構：**
```
┌─────────────────────────────────────┐
│  App Shell (應用外殼)                │
│  ┌───────────────────────────────┐  │
│  │  Status Bar (系統狀態列)       │  │
│  │  - iOS: 44px (含安全區域)     │  │
│  │  - Android: 24px (不含安全區域)│  │
│  └───────────────────────────────┘  │
│  ┌───────────────────────────────┐  │
│  │  Navigation Container         │  │
│  │  ┌─────────────────────────┐  │  │
│  │  │  Top Navigation Bar     │  │  │
│  │  │  (56px)                 │  │  │
│  │  └─────────────────────────┘  │  │
│  │  ┌─────────────────────────┐  │  │
│  │  │  Content Area           │  │  │
│  │  │  (可滾動，動態高度)       │  │  │
│  │  └─────────────────────────┘  │  │
│  │  ┌─────────────────────────┐  │  │
│  │  │  Bottom Navigation Bar  │  │  │
│  │  │  (60px + 安全區域)       │  │  │
│  │  └─────────────────────────┘  │  │
│  └───────────────────────────────┘  │
│  ┌───────────────────────────────┐  │
│  │  Overlay Layer (覆蓋層)        │  │
│  │  - 模態框、Toast、載入提示     │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

### 3.2 安全區域處理（Safe Area）

**iOS 安全區域：**
- 頂部：狀態列高度 + 安全區域（iPhone X 以上）
- 底部：底部導航高度 + 安全區域（Home Indicator）
- 使用 `SafeArea` widget 包裹內容

**Android 安全區域：**
- 頂部：狀態列高度（通常 24px）
- 底部：導航列高度（如有實體按鈕則為 0）
- 使用 `MediaQuery.of(context).padding` 處理

**實作建議：**
```dart
// Flutter 實作範例
SafeArea(
  child: Scaffold(
    body: Column(
      children: [
        TopNavigationBar(),
        Expanded(
          child: ContentArea(),
        ),
        BottomNavigationBar(),
      ],
    ),
  ),
)
```

### 3.3 狀態列處理（Status Bar）

**iOS 狀態列：**
- 高度：44px（含安全區域）
- 樣式：深色內容（預設）或淺色內容
- 自動適配：根據背景色自動調整

**Android 狀態列：**
- 高度：24px（不含安全區域）
- 樣式：使用 `SystemUiOverlayStyle` 設定
- 顏色：可設定為透明或半透明

**實作建議：**
```dart
// 設定狀態列樣式
SystemChrome.setSystemUIOverlayStyle(
  SystemUiOverlayStyle(
    statusBarColor: Colors.transparent, // Android
    statusBarIconBrightness: Brightness.dark, // Android
    statusBarBrightness: Brightness.light, // iOS
  ),
);
```

### 3.4 標準頁面結構

**基礎頁面模板：**
```
┌─────────────────────────┐
│  Status Bar (系統)       │
├─────────────────────────┤
│  Top Navigation Bar     │
│  (56px)                 │
│  [返回] 標題      [操作]  │
├─────────────────────────┤
│                          │
│  Content Area            │
│  (可滾動，動態高度)       │
│  - Padding: 16px        │
│  - 垂直滾動              │
│                          │
│                          │
├─────────────────────────┤
│  Bottom Navigation Bar   │
│  (60px + 安全區域)       │
│  [首頁][分類][發布][通知][我的]│
└─────────────────────────┘
```

### 3.5 頁面類型與佈局

#### 3.5.1 列表頁（List Page）

**適用場景：**
- 首頁商品列表
- 分類商品列表
- 搜尋結果頁
- 我的商品列表
- 交易紀錄列表

**佈局結構：**
```
┌─────────────────────────┐
│  Top Navigation Bar     │
│  [返回] 首頁    [搜尋]   │
├─────────────────────────┤
│  Content Area           │
│  ┌───────────────────┐  │
│  │  [商品卡片]        │  │
│  │  (間距: 16px)     │  │
│  ├───────────────────┤  │
│  │  [商品卡片]        │  │
│  ├───────────────────┤  │
│  │  [商品卡片]        │  │
│  │  ...              │  │
│  └───────────────────┘  │
│  (垂直滾動，無限載入)     │
├─────────────────────────┤
│  Bottom Navigation Bar   │
└─────────────────────────┘
```

**特點：**
- 內容區域可垂直滾動
- 支援下拉刷新（Pull to Refresh）
- 支援上拉載入更多（Infinite Scroll）
- 卡片之間間距：`16px`
- 頁面左右邊距：`16px`

#### 3.5.2 詳情頁（Detail Page）

**適用場景：**
- 商品詳情頁
- 個人主頁
- 交易詳情頁

**佈局結構：**
```
┌─────────────────────────┐
│  Top Navigation Bar     │
│  [返回] 商品詳情  [更多] │
├─────────────────────────┤
│  Content Area           │
│  ┌───────────────────┐  │
│  │  [圖片輪播]        │  │
│  ├───────────────────┤  │
│  │  [商品資訊]        │  │
│  ├───────────────────┤  │
│  │  [賣家資訊]        │  │
│  ├───────────────────┤  │
│  │  [留言區]          │  │
│  └───────────────────┘  │
│  (垂直滾動)              │
├─────────────────────────┤
│  Fixed Bottom Bar        │
│  [留言] [議價] [購買]    │
│  (高度: 60px)            │
├─────────────────────────┤
│  Bottom Navigation Bar   │
└─────────────────────────┘
```

**特點：**
- 內容區域可垂直滾動
- 固定底部操作欄（不隨內容滾動）
- 固定底部欄高度：`60px`
- 固定底部欄背景：`#FFFFFF`，頂部邊框：`1px solid Border-Light`
- 固定底部欄陰影：`Small Shadow`

#### 3.5.3 表單頁（Form Page）

**適用場景：**
- 發布商品頁
- 編輯個人資料頁
- 設定頁

**佈局結構：**
```
┌─────────────────────────┐
│  Top Navigation Bar     │
│  [返回] 發布商品         │
├─────────────────────────┤
│  Content Area           │
│  ┌───────────────────┐  │
│  │  [表單欄位 1]      │  │
│  ├───────────────────┤  │
│  │  [表單欄位 2]      │  │
│  ├───────────────────┤  │
│  │  [表單欄位 3]      │  │
│  │  ...              │  │
│  └───────────────────┘  │
│  (垂直滾動，鍵盤彈出時自動調整)│
├─────────────────────────┤
│  Fixed Bottom Bar        │
│  [發布商品]              │
│  (高度: 60px)            │
├─────────────────────────┤
│  Bottom Navigation Bar   │
└─────────────────────────┘
```

**特點：**
- 內容區域可垂直滾動
- 鍵盤彈出時自動調整內容區域高度
- 固定底部提交按鈕
- 表單欄位之間間距：`20px`
- 表單標籤與輸入框間距：`8px`

#### 3.5.4 全螢幕頁（Full Screen Page）

**適用場景：**
- 圖片全螢幕查看
- 引導頁
- 登入/註冊頁

**佈局結構：**
```
┌─────────────────────────┐
│  Status Bar (系統)       │
├─────────────────────────┤
│  Content Area           │
│  (全螢幕，無導航欄)       │
│                          │
│                          │
│                          │
│                          │
└─────────────────────────┘
```

**特點：**
- 隱藏頂部導航欄
- 隱藏底部導航欄
- 內容佔滿整個螢幕
- 使用 `SafeArea` 處理安全區域

### 3.6 導航系統詳細定義

#### 3.6.1 頂部導航欄（Top Navigation Bar）

**結構規範：**
```
┌─────────────────────────────────────┐
│  [返回]  [標題]          [操作]     │
│  24px    居中對齊        24px       │
│  左側     中間           右側       │
└─────────────────────────────────────┘
```

**尺寸規範：**
- 高度：`56px`
- 背景色：`#FFFFFF`
- 邊框：`1px solid Border-Light`（底部）
- 內邊距：左右各 `16px`

**左側區域：**
- 返回按鈕：`48x48px` 觸控區域，`24x24px` 圖示
- 圖示：向左箭頭（←）
- 顏色：`Text-Primary` (#1E293B)

**中間區域：**
- 頁面標題：`18px`，粗體 `600`
- 顏色：`Text-Primary` (#1E293B)
- 對齊：居中
- 最大寬度：`60%`（避免與左右按鈕重疊）

**右側區域：**
- 操作按鈕：`48x48px` 觸控區域，`24x24px` 圖示
- 可選操作：分享、更多選項、搜尋等
- 多個按鈕時，間距：`8px`

**特殊狀態：**
- **透明模式**：用於圖片全螢幕查看
  - 背景：透明或半透明
  - 文字/圖示：白色
- **滾動隱藏**：詳情頁向下滾動時隱藏（可選）

#### 3.6.2 底部導航欄（Bottom Navigation Bar）

**結構規範：**
```
┌─────────────────────────────────────┐
│  [首頁] [分類] [發布] [通知] [我的] │
│  圖示   圖示   圖示   圖示   圖示   │
│  文字   文字   文字   文字   文字   │
└─────────────────────────────────────┘
```

**尺寸規範：**
- 高度：`60px` + 安全區域（iOS）
- 背景色：`#FFFFFF`
- 邊框：`1px solid Border-Light`（頂部）
- 陰影：`Small Shadow`（向上）
- 內邊距：上下各 `8px`

**導航項目：**
1. **首頁（Home）**
   - 圖示：🏠 或 house icon
   - 文字：「首頁」
   - 路由：`/home`

2. **分類（Categories）**
   - 圖示：📂 或 grid icon
   - 文字：「分類」
   - 路由：`/categories`

3. **發布（Publish）**
   - 圖示：➕ 或 add icon（較大，突出顯示）
   - 文字：「發布」
   - 路由：`/publish`
   - 特殊：可選用浮動按鈕（FAB）樣式

4. **通知（Notifications）**
   - 圖示：🔔 或 bell icon
   - 文字：「通知」
   - 路由：`/notifications`
   - 徽章：未讀通知數量（紅色圓點，`8x8px`）

5. **我的（Profile）**
   - 圖示：👤 或 user icon
   - 文字：「我的」
   - 路由：`/profile`

**圖示規範：**
- 尺寸：`24x24px`
- 未選中：`Text-Secondary` (#64748B)
- 選中：`Primary-500` (#2563EB)
- 間距：圖示與文字之間 `4px`

**文字規範：**
- 字體大小：`12px`
- 字體粗細：`Medium (500)`
- 未選中：`Text-Secondary` (#64748B)
- 選中：`Primary-500` (#2563EB)
- 位置：圖示下方

**互動狀態：**
- **Default**：正常顯示
- **Selected**：圖示和文字變為 `Primary-500`，可選加粗
- **Pressed**：輕微縮放 `0.95`，`100ms`

**特殊處理：**
- **徽章顯示**：通知項目顯示未讀數量
  - 位置：圖示右上角
  - 尺寸：`16x16px`（單數字）或 `20x20px`（雙數字）
  - 背景：`Error-500` (#EF4444)
  - 文字：白色，`10px`，粗體
  - 圓角：`50%`

### 3.7 鍵盤處理（Keyboard Handling）

**鍵盤彈出行為：**
- 表單頁：內容區域自動調整，確保輸入框可見
- 使用 `Scaffold` 的 `resizeToAvoidBottomInset: true`
- 鍵盤彈出動畫：`300ms ease-in-out`

**鍵盤關閉：**
- 點擊輸入框外部：自動關閉鍵盤
- 使用 `GestureDetector` 包裹內容區域
- 點擊空白處：`FocusScope.of(context).unfocus()`

**實作建議：**
```dart
Scaffold(
  resizeToAvoidBottomInset: true,
  body: GestureDetector(
    onTap: () => FocusScope.of(context).unfocus(),
    child: ContentArea(),
  ),
)
```

### 3.8 滾動行為（Scroll Behavior）

**垂直滾動：**
- 使用 `SingleChildScrollView` 或 `ListView`
- 滾動物理效果：`ClampingScrollPhysics`（iOS）或 `BouncingScrollPhysics`（Android）
- 滾動條：隱藏（預設）或顯示（可選）

**下拉刷新：**
- 使用 `RefreshIndicator`
- 顏色：`Primary-500`
- 觸發距離：`80px`
- 動畫時長：`300ms`

**上拉載入：**
- 使用 `ListView` 的 `onScrollEnd` 或 `ScrollController`
- 觸發距離：距離底部 `200px`
- 顯示載入指示器：旋轉載入或骨架屏

### 3.9 模態框與覆蓋層（Modals & Overlays）

**模態框層級：**
- 位於所有頁面內容之上
- 背景遮罩：`Background-Overlay` (60% 透明度黑色)
- 點擊背景：關閉模態框
- 滑動手勢：向下滑動關閉（可選）

**Toast 提示：**
- 位置：頂部或底部
- 顯示時長：`2秒`（成功）或 `4秒`（錯誤）
- 動畫：淡入淡出，`200ms`

**載入提示：**
- 全螢幕載入：顯示在內容區域上方
- 按鈕載入：顯示在按鈕內部
- 頁面載入：使用骨架屏

### 3.10 Flutter 實作建議

**使用 Scaffold 作為基礎：**
```dart
Scaffold(
  // 狀態列樣式
  backgroundColor: Colors.white,
  
  // 頂部導航欄
  appBar: AppBar(
    elevation: 0,
    backgroundColor: Colors.white,
    leading: BackButton(),
    title: Text('頁面標題'),
    actions: [ActionButton()],
  ),
  
  // 內容區域
  body: SafeArea(
    child: ContentArea(),
  ),
  
  // 底部導航欄
  bottomNavigationBar: BottomNavigationBar(
    type: BottomNavigationBarType.fixed,
    items: navigationItems,
  ),
  
  // 固定底部欄（可選）
  persistentFooterButtons: [
    FixedBottomButton(),
  ],
)
```

**使用 go_router 管理路由：**
```dart
final router = GoRouter(
  routes: [
    GoRoute(
      path: '/home',
      builder: (context, state) => HomePage(),
    ),
    // ... 其他路由
  ],
);
```

**使用 Riverpod 管理狀態：**
```dart
// 導航狀態管理
final currentIndexProvider = StateProvider<int>((ref) => 0);

// 頁面狀態管理
final pageStateProvider = StateNotifierProvider<PageStateNotifier, PageState>(
  (ref) => PageStateNotifier(),
);
```

### 3.11 響應式佈局適配

**手機（320px - 767px）：**
- 單欄佈局
- 底部導航欄：5 個項目（可橫向滾動，如需要）
- 卡片寬度：`100%`（減去左右邊距）

**平板（768px - 1023px）：**
- 可選 2 欄佈局（商品列表）
- 底部導航欄：保持 5 個項目
- 卡片寬度：`48%`（2 欄）或 `32%`（3 欄）

**桌面（1024px+）：**
- 最大寬度：`1200px`，置中顯示
- 可選側邊欄導航（替代底部導航）
- 卡片寬度：`25%`（4 欄）

### 3.12 頁面轉場動畫

**推入動畫（Push）：**
- 新頁面從右側滑入
- 時長：`300ms`
- 曲線：`ease-in-out`

**彈出動畫（Pop）：**
- 當前頁面向右滑出
- 時長：`300ms`
- 曲線：`ease-in-out`

**模態框動畫：**
- 從底部向上滑入
- 時長：`300ms`
- 曲線：`ease-out`

**實作建議：**
```dart
// 使用 go_router 的頁面轉場
GoRoute(
  path: '/detail',
  pageBuilder: (context, state) => CustomTransitionPage(
    child: DetailPage(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return SlideTransition(
        position: animation.drive(
          Tween(begin: Offset(1.0, 0.0), end: Offset.zero)
            .chain(CurveTween(curve: Curves.easeInOut)),
        ),
        child: child,
      );
    },
  ),
)
```

---

## 4. 導航系統（Navigation System）

### 4.1 底部導航（Bottom Navigation）

**樣式規範：**
- 高度：`60px`
- 背景色：`#FFFFFF`
- 邊框：`1px solid Border-Light`（頂部）
- 陰影：`Small Shadow`

**導航項目：**
- 首頁（Home）
- 分類（Categories）
- 發布（Publish）
- 通知（Notifications）
- 我的（Profile）

**圖示規範：**
- 尺寸：`24x24px`
- 未選中：`Text-Secondary` (#64748B)
- 選中：`Primary-500` (#2563EB)
- 文字：`12px`，位於圖示下方

### 4.2 頂部導航（Top Navigation）

**樣式規範：**
- 高度：`56px`
- 背景色：`#FFFFFF`
- 邊框：`1px solid Border-Light`（底部）

**內容：**
- 左側：返回按鈕（24x24px）
- 中間：頁面標題（18px，粗體 600）
- 右側：操作按鈕（可選）

---

## 5. 互動設計（Interaction Design）

### 5.1 觸控目標（Touch Targets）

**最小尺寸：**
- 按鈕：`48x48px`（符合無障礙標準）
- 圖示按鈕：`48x48px`
- 列表項目：最小高度 `56px`
- 輸入框：高度 `48px`

**間距：**
- 觸控目標之間：最小 `8px`

### 5.2 手勢支援（Gestures）

**支援的手勢：**
- **點擊（Tap）**：主要互動方式
- **長按（Long Press）**：顯示上下文選單
- **滑動（Swipe）**：圖片輪播、刪除操作
- **下拉刷新（Pull to Refresh）**：列表頁面
- **上拉載入（Pull to Load）**：無限滾動

### 5.3 狀態反饋（Feedback）

**視覺反饋：**
- 按鈕點擊：縮放 `0.95`，`100ms`
- 卡片點擊：縮放 `0.98`，`200ms`
- 載入狀態：顯示骨架屏或載入動畫
- 成功狀態：顯示成功提示（2秒後自動消失）
- 錯誤狀態：顯示錯誤提示（需手動關閉）

**觸覺反饋（Haptic Feedback）：**
- 按鈕點擊：輕微震動（iOS）
- 成功操作：輕微震動
- 錯誤操作：中等震動

---

## 6. 響應式設計（Responsive Design）

### 6.1 斷點（Breakpoints）

**手機（Mobile）：**
- 寬度：`320px - 767px`
- 主要設計目標

**平板（Tablet）：**
- 寬度：`768px - 1023px`
- 調整卡片佈局（2-3 欄）

**桌面（Desktop）：**
- 寬度：`1024px+`
- 最大寬度：`1200px`，置中顯示

### 6.2 適配策略

**手機優先（Mobile First）：**
- 所有設計以手機為基準
- 平板和桌面在此基礎上擴展

**佈局調整：**
- 手機：單欄佈局
- 平板：2-3 欄佈局
- 桌面：3-4 欄佈局

---

## 7. 無障礙設計（Accessibility）

### 7.1 對比度（Contrast）

**文字對比度：**
- 主要文字與背景：≥ 4.5:1（WCAG AA）
- 重要文字與背景：≥ 7:1（WCAG AAA）

**非文字對比度：**
- 圖示與背景：≥ 3:1（WCAG AA）
- 按鈕與背景：≥ 3:1（WCAG AA）

### 7.2 字體大小

**最小字體：**
- 正文：`14px`（最小）
- 重要資訊：`16px` 以上
- 中高齡友善：關鍵資訊 `18px` 以上

**可調整性：**
- 支援系統字體大小設定
- 動態調整 UI 元素大小

### 7.3 觸控目標

**最小尺寸：**
- 所有可點擊元素：`48x48px`
- 列表項目：最小高度 `56px`

### 7.4 語義標籤

**使用語義化標籤：**
- 按鈕：使用 `<button>` 標籤
- 連結：使用 `<a>` 標籤
- 表單：使用正確的 `<input>` 類型
- 標題：使用正確的標題層級（H1-H4）

### 7.5 螢幕閱讀器支援

**ARIA 標籤：**
- 為互動元素添加 `aria-label`
- 為狀態元素添加 `aria-live`
- 為表單添加 `aria-describedby`

**焦點管理：**
- 清晰的焦點指示器
- 邏輯的焦點順序
- 鍵盤導航支援

---

## 8. 動畫與過渡（Animations & Transitions）

### 8.1 頁面轉場（Page Transitions）

**轉場類型：**
- **推入（Push）**：新頁面從右側滑入，`300ms ease-in-out`
- **彈出（Pop）**：返回時頁面向右滑出，`300ms ease-in-out`
- **淡入淡出（Fade）**：模態框顯示/隱藏，`200ms ease-in-out`

### 8.2 元素動畫（Element Animations）

**出現動畫：**
- 列表項目：從下往上淡入，`300ms ease-out`
- 卡片：淡入 + 輕微上移，`300ms ease-out`
- 模態框：淡入 + 輕微上移，`300ms ease-out`

**互動動畫：**
- 按鈕點擊：縮放 `0.95`，`100ms ease-out`
- 卡片點擊：縮放 `0.98`，`200ms ease-out`
- 開關切換：滑動動畫，`200ms ease-in-out`

---

## 9. 圖示系統（Icon System）

### 9.1 圖示風格

**風格選擇：**
- 使用 **線性圖示（Outline Icons）** 作為主要風格
- 圖示線條粗細：`2px`
- 圓角：輕微圓角（`2px`）

**圖示庫：**
- 推薦使用 `Material Icons` 或 `Feather Icons`
- 確保圖示風格一致

### 9.2 圖示尺寸

**標準尺寸：**
- `Small`: `16x16px` - 內聯圖示
- `Medium`: `24x24px` - 標準圖示（最常用）
- `Large`: `32x32px` - 大圖示
- `XLarge`: `48x48px` - 超大圖示

### 9.3 圖示使用規範

**顏色：**
- 預設：`Text-Secondary` (#64748B)
- 選中：`Primary-500` (#2563EB)
- 禁用：`Text-Disabled` (#CBD5E1)

**使用場景：**
- 導航圖示：使用 `24x24px`
- 操作圖示：使用 `24x24px`
- 裝飾圖示：使用 `16x16px` 或 `32x32px`

---

## 10. 圖片規範（Image Guidelines）

### 10.1 商品圖片

**尺寸要求：**
- 最小尺寸：`800x800px`
- 推薦尺寸：`1200x1200px`
- 最大尺寸：`2000x2000px`
- 格式：`JPEG` 或 `PNG`
- 檔案大小：單張 < 1MB（上傳前自動壓縮）

**比例：**
- 列表卡片：`16:9`（橫向）
- 詳情頁：`1:1`（正方形）
- 縮圖：`4:3`

### 10.2 圖片處理

**上傳處理：**
- 自動壓縮：單張圖片 < 1MB
- 自動裁切：統一為正方形（詳情頁）
- 自動優化：降低品質但保持視覺效果

**顯示處理：**
- 漸進式載入：先顯示模糊版本，再載入清晰版本
- 懶加載：只載入可見區域的圖片
- 快取：已載入的圖片快取到本地

---

## 11. 錯誤處理設計（Error Handling Design）

### 11.1 錯誤訊息設計

**樣式規範：**
- 背景色：`Error-50` (#FEF2F2)
- 邊框：`1px solid Error-500`
- 文字顏色：`Error-500`
- 圖示：⚠️ 警告圖示
- 內邊距：`12px 16px`
- 圓角：`8px`

**內容要求：**
- 清楚說明問題
- 提供解決方法
- 使用友善的語言（避免技術術語）

### 11.2 錯誤類型

**網路錯誤：**
- 訊息：「網路連線不穩，請稍後再試」
- 操作：提供「重試」按鈕

**驗證錯誤：**
- 訊息：「請檢查輸入的資料是否正確」
- 操作：標示錯誤欄位

**權限錯誤：**
- 訊息：「此功能需要完成社區驗證」
- 操作：提供「前往驗證」按鈕

**系統錯誤：**
- 訊息：「系統發生錯誤，我們正在處理中」
- 操作：提供「回報問題」連結

---

## 12. 空狀態設計（Empty States）

### 12.1 空狀態類型

**無商品：**
- 圖示：📦 空盒子圖示
- 標題：「還沒有商品」
- 描述：「成為第一個發布商品的人吧！」
- 操作：提供「發布商品」按鈕

**無搜尋結果：**
- 圖示：🔍 搜尋圖示
- 標題：「找不到相關商品」
- 描述：「試試其他關鍵字或調整篩選條件」
- 操作：提供「清除搜尋」按鈕

**無通知：**
- 圖示：🔔 通知圖示
- 標題：「還沒有通知」
- 描述：「當有新的留言或交易時，會在這裡通知您」

---

## 13. 載入狀態設計（Loading States）

### 13.1 載入類型

**頁面載入：**
- 使用骨架屏（Skeleton Screen）
- 顯示頁面結構的佔位符

**按鈕載入：**
- 顯示旋轉載入動畫
- 按鈕文字改為「處理中...」
- 禁用按鈕點擊

**列表載入：**
- 顯示骨架屏卡片
- 底部顯示「載入中...」

**圖片載入：**
- 顯示模糊的佔位圖
- 漸進式載入清晰版本

---

## 14. 實作指南（Implementation Guidelines）

### 14.1 Flutter 實作建議

**使用 Material Design 3：**
- Flutter 3.16+ 支援 Material Design 3
- 使用 `ThemeData` 定義設計系統

**組件實作：**
- 使用 `flutter_riverpod` 管理狀態
- 建立可重用的組件庫
- 使用 `flutter_screenutil` 適配不同螢幕尺寸

**動畫實作：**
- 使用 `AnimatedContainer` 實現過渡動畫
- 使用 `Hero` 實現頁面轉場動畫
- 使用 `shimmer` 實現骨架屏效果

### 14.2 設計 Token 管理

**建議使用：**
- 建立 `design_tokens.dart` 文件
- 定義所有顏色、字體、間距、圓角等 Token
- 使用 `ThemeData` 統一管理

**範例結構：**
```dart
class DesignTokens {
  // Colors
  static const Color primary500 = Color(0xFF2563EB);
  static const Color textPrimary = Color(0xFF1E293B);
  
  // Spacing
  static const double space4 = 16.0;
  static const double space6 = 24.0;
  
  // Typography
  static const double fontSize16 = 16.0;
  static const FontWeight fontWeightMedium = FontWeight.w500;
}
```

---

## 15. 設計檢查清單（Design Checklist）

### 15.1 視覺檢查

- [ ] 所有顏色符合設計系統
- [ ] 所有字體大小符合規範
- [ ] 所有間距符合 8px 網格系統
- [ ] 所有圓角符合規範
- [ ] 所有陰影符合規範
- [ ] 圖片比例正確
- [ ] 圖示風格一致

### 15.2 互動檢查

- [ ] 所有按鈕有明確的狀態反饋
- [ ] 所有觸控目標 ≥ 48x48px
- [ ] 所有動畫時長適中（不超過 500ms）
- [ ] 載入狀態清晰可見
- [ ] 錯誤訊息友善易懂

### 15.3 無障礙檢查

- [ ] 文字對比度 ≥ 4.5:1
- [ ] 重要資訊字體 ≥ 16px
- [ ] 所有互動元素有語義標籤
- [ ] 支援鍵盤導航
- [ ] 支援螢幕閱讀器

### 15.4 響應式檢查

- [ ] 手機佈局正常顯示
- [ ] 平板佈局正常顯示（如適用）
- [ ] 不同螢幕尺寸適配良好
- [ ] 橫向/縱向切換正常

---

## 附錄：相關文件

- [PRD.md](../PRD.md) - 產品需求文件
- [ux-checkout-process.md](./ux-checkout-process.md) - 結帳流程 UX 設計
- [ux-checkout-process-wireframes.html](./ux-checkout-process-wireframes.html) - 結帳流程 Wireframes
- [epics.md](./epics.md) - Epic 分解文件

---

## 版本歷史

**Version 1.0 (2025-11-13)**
- 初始版本
- 定義完整的設計系統
- 建立組件庫規範
- 定義互動設計和無障礙設計

---

_本文件為鄰里市集的 UI 設計規範，供開發團隊實作參考。設計系統會根據實際使用情況持續優化。_

