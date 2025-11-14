# 鄰里市集 - Epic Breakdown

**Author:** yyx
**Date:** 2025-11-13
**Project Level:** Level 2 (Medium Complexity)
**Target Scale:** MVP → Growth → Vision

---

## Overview

本文件提供鄰里市集的完整 Epic 和 Story 分解，將 [PRD](./PRD.md) 中的 34 個功能需求轉換為可實作的故事。

### Epic 結構總覽

本專案包含 **10 個 Epic**，涵蓋從基礎建設到使用者體驗優化的完整功能範圍：

1. **Epic 1: 專案基礎建設** - 建立開發環境和基礎架構
2. **Epic 2: 使用者認證與身份驗證** - 使用者註冊、登入和社區驗證
3. **Epic 3: 商品管理核心功能** - 商品發布、瀏覽、搜尋和管理
4. **Epic 4: 溝通與互動系統** - 留言板和通知系統
5. **Epic 5: 交易流程** - 交易確認、彈性交易模式和錯誤處理
6. **Epic 6: 信任與評價系統** - 評價、個人主頁和交易紀錄
7. **Epic 7: 安全與內容管理** - 檢舉、封鎖和爭議處理
8. **Epic 8: 管理室後台系統** - Web 後台管理功能
9. **Epic 9: 數據隱私與合規** - 隱私保護和個資法合規
10. **Epic 10: 使用者體驗優化** - 引導、錯誤處理和離線功能

---

## Epic 1: 專案基礎建設

**目標：** 建立 Flutter 專案基礎架構、Supabase 後端設定、開發環境和部署管道，為後續所有功能開發奠定基礎。

### Story 1.1: Flutter 專案初始化與基礎架構設置

As a **開發者**,
I want **建立 Flutter 專案並配置基礎架構**,
So that **後續功能開發有穩固的基礎**。

**Acceptance Criteria:**

**Given** 開發環境已準備就緒（Flutter SDK、Dart、IDE）
**When** 初始化 Flutter 專案
**Then** 專案結構符合最佳實踐，包含：
- 清晰的資料夾結構（lib/features, lib/core, lib/shared）
- pubspec.yaml 配置基本依賴
- 基本的導航結構（go_router）
- 狀態管理架構（Riverpod）

**And** 專案可以成功編譯和運行

**Prerequisites:** 無（這是第一個 Story）

**Technical Notes:**
- 使用 Flutter 3.16+ 和 Dart 3.2+
- 設定 Riverpod 作為狀態管理
- 設定 go_router 作為導航
- 建立基本的專案結構和命名規範
- 配置 linting 規則（flutter_lints）

---

### Story 1.2: Supabase 後端服務設定

As a **開發者**,
I want **設定 Supabase 後端服務**,
So that **應用程式可以連接資料庫、認證和儲存服務**。

**Acceptance Criteria:**

**Given** Supabase 專案已建立
**When** 配置 Supabase Flutter SDK
**Then** 應用程式可以：
- 連接到 Supabase 專案
- 使用 Supabase Auth 進行認證
- 存取 Supabase Database
- 使用 Supabase Storage 上傳檔案

**And** 環境變數正確配置（API Key、URL）

**Prerequisites:** Story 1.1

**Technical Notes:**
- 設定 Supabase Flutter SDK (supabase_flutter)
- 配置環境變數管理（flutter_dotenv）
- 建立 Supabase 服務類別（單例模式）
- 設定 Row Level Security (RLS) 基礎架構

---

### Story 1.3: 資料庫結構設計與建立

As a **開發者**,
I want **設計和建立資料庫結構**,
So that **所有功能模組都有對應的資料表**。

**Acceptance Criteria:**

**Given** Supabase 資料庫已連接
**When** 建立所有必要的資料表
**Then** 資料庫包含以下資料表：
- users（使用者資料）
- communities（社區資料）
- community_members（社區成員關聯）
- items（商品資料）
- item_images（商品圖片）
- comments（留言資料）
- transactions（交易資料）
- reviews（評價資料）
- notifications（通知資料）
- reports（檢舉資料）
- admin_logs（管理員操作日誌）

**And** 所有資料表都有適當的索引和外鍵約束
**And** Row Level Security (RLS) 政策已設定

**Prerequisites:** Story 1.2

**Technical Notes:**
- 使用 Supabase SQL Editor 建立資料表
- 設計資料表關聯（Foreign Keys）
- 設定索引以優化查詢效能
- 建立 RLS 政策確保資料安全
- 建立資料庫遷移腳本

---

### Story 1.4: 圖片上傳與儲存功能

As a **開發者**,
I want **實作圖片上傳和儲存功能**,
So that **使用者可以上傳商品照片**。

**Acceptance Criteria:**

**Given** 使用者已登入
**When** 選擇圖片（拍照或相簿）
**Then** 圖片自動壓縮（單張 < 1MB）
**And** 圖片上傳到 Supabase Storage
**And** 上傳成功後返回圖片 URL
**And** 顯示上傳進度

**Prerequisites:** Story 1.2, Story 1.3

**Technical Notes:**
- 使用 image_picker 選擇圖片
- 使用 image_cropper 裁切圖片
- 實作圖片壓縮功能
- 設定 Supabase Storage bucket 和權限
- 處理上傳錯誤和重試機制

---

### Story 1.5: 推播通知基礎架構

As a **開發者**,
I want **設定推播通知基礎架構**,
So that **應用程式可以發送推播通知**。

**Acceptance Criteria:**

**Given** Firebase Cloud Messaging (FCM) 已設定
**When** 應用程式啟動
**Then** 應用程式可以：
- 獲取 FCM Token
- 將 Token 儲存到 Supabase
- 接收推播通知
- 處理背景和前景通知

**And** iOS 和 Android 都支援推播通知

**Prerequisites:** Story 1.2

**Technical Notes:**
- 設定 Firebase Cloud Messaging
- 配置 iOS APNs 憑證
- 配置 Android google-services.json
- 實作 FCM Token 管理
- 建立 Supabase Edge Function 處理推播邏輯

---

## Epic 2: 使用者認證與身份驗證

**目標：** 讓使用者可以快速註冊、登入，並透過社區管理室驗證身份，建立信任基礎。

### Story 2.1: 第三方登入功能（Google/Apple/LINE）

As a **新使用者**,
I want **使用 Google、Apple 或 LINE 帳號登入**,
So that **我可以快速註冊和登入，無需記憶密碼**。

**Acceptance Criteria:**

**Given** 使用者開啟應用程式
**When** 選擇第三方登入方式（Google/Apple/LINE）
**Then** 系統導向對應的登入頁面
**And** 登入成功後自動建立帳號
**And** 第三方資料（姓名、Email、頭像）自動匯入
**And** 登入狀態持久保存

**Prerequisites:** Story 1.2

**Technical Notes:**
- 設定 Google Sign-In (google_sign_in)
- 設定 Apple Sign-In (sign_in_with_apple) - iOS 必備
- 設定 LINE Login (flutter_line_sdk)
- 整合 Supabase Auth
- 處理登入錯誤和例外情況

---

### Story 2.2: 個人資料管理

As a **已登入使用者**,
I want **設定和編輯我的個人資料**,
So that **其他使用者可以了解我的基本資訊**。

**Acceptance Criteria:**

**Given** 使用者已登入
**When** 進入個人資料設定頁面
**Then** 可以編輯：
- 暱稱（2-20字元，同社區內不可重複）
- 頭像（支援拍照或相簿選擇，自動裁切為正方形）
- 個人簡介（選填，50字內）

**And** 系統自動顯示：信用評分、交易次數、加入日期、已加入的社區列表
**And** 資料儲存成功後顯示確認訊息

**Prerequisites:** Story 2.1, Story 1.3

**Technical Notes:**
- 建立個人資料編輯 UI
- 實作暱稱重複檢查（同社區內）
- 整合圖片上傳功能（Story 1.4）
- 更新 Supabase users 資料表

---

### Story 2.3: 管理員後台 - 社區與門牌管理

As a **管理員**,
I want **在 Web 後台管理社區和門牌資料**,
So that **我可以為住戶產生驗證 QR Code**。

**Acceptance Criteria:**

**Given** 管理員已登入後台
**When** 進入社區管理介面
**Then** 可以：
- 新增/編輯社區資訊（社區名稱、地址、建築數量）
- 建立建築結構（A棟、B棟、C棟）
- 設定樓層和門牌號碼
- 批次匯入門牌資料（CSV上傳）

**And** 門牌狀態管理（已驗證/未驗證/已出租/空屋）

**Prerequisites:** Story 1.2, Story 1.3

**Technical Notes:**
- 建立 React/Vue.js Web 後台
- 實作管理員認證系統
- 建立社區和門牌管理 UI
- 實作 CSV 匯入功能
- 設定管理員權限（RLS）

---

### Story 2.4: 管理員後台 - QR Code 產生器

As a **管理員**,
I want **為特定門牌產生驗證 QR Code**,
So that **住戶可以掃描 QR Code 完成社區驗證**。

**Acceptance Criteria:**

**Given** 管理員已選擇社區和門牌
**When** 點擊「產生 QR Code」
**Then** 系統產生包含以下資訊的 QR Code：
- 社區 ID 和名稱
- 建築、樓層、門牌號碼
- 一次性驗證 Token（5分鐘有效）

**And** QR Code 即時顯示（可全螢幕放大）
**And** 顯示有效期限倒數計時（5分鐘）
**And** 可下載或列印 QR Code

**Prerequisites:** Story 2.3

**Technical Notes:**
- 使用 qrcode.js 產生 QR Code
- 實作一次性 Token 生成機制
- 設定 Token 有效期限（5分鐘）
- 實作 QR Code 顯示和下載功能

---

### Story 2.5: 社區身份驗證（QR Code 掃描與綁定）

As a **已登入使用者**,
I want **掃描管理室提供的 QR Code 完成社區驗證**,
So that **我可以解鎖完整功能並建立社區身份**。

**Acceptance Criteria:**

**Given** 使用者已登入但未完成社區驗證
**When** 前往管理室掃描 QR Code
**Then** 系統自動帶入社區資訊和門牌號碼
**And** 使用者確認資訊後完成綁定
**And** 顯示「歡迎加入【XX社區】XX號」
**And** 驗證 Token 使用後立即失效
**And** 驗證記錄保存到資料庫（包含 IP 位址和時間戳記）

**Prerequisites:** Story 2.1, Story 2.4

**Technical Notes:**
- 實作 QR Code 掃描功能（qr_code_scanner）
- 解析 QR Code 內容（JSON 格式）
- 驗證 Token 有效性和一次性使用
- 更新 community_members 資料表
- 處理驗證錯誤（過期、已使用等）

---

### Story 2.6: 社區認證徽章顯示

As a **已驗證使用者**,
I want **顯示社區認證徽章**,
So that **其他使用者知道我是已驗證的社區成員**。

**Acceptance Criteria:**

**Given** 使用者已完成社區驗證
**When** 其他使用者查看我的資訊
**Then** 在以下位置顯示認證徽章（藍色勾勾）：
- 個人主頁
- 商品詳情的賣家資訊
- 留言區的留言者名稱旁
- 通知列表

**And** 滑鼠 hover 顯示「已驗證社區成員」tooltip

**Prerequisites:** Story 2.5

**Technical Notes:**
- 建立認證徽章元件
- 在相關 UI 位置整合徽章顯示
- 根據驗證狀態動態顯示/隱藏

---

### Story 2.7: 首次使用引導流程

As a **新使用者**,
I want **看到首次使用引導**,
So that **我可以快速了解平台功能和使用方式**。

**Acceptance Criteria:**

**Given** 使用者首次開啟應用程式
**When** 完成登入
**Then** 顯示引導流程：
- 歡迎頁面（介紹平台核心價值）
- 登入引導（說明第三方登入方式）
- 社區驗證引導（說明驗證重要性）
- 功能介紹（首次使用各功能時顯示）

**And** 使用者可選擇「跳過引導」
**And** 引導可隨時在設定中重新開啟

**Prerequisites:** Story 2.1

**Technical Notes:**
- 建立引導流程 UI（使用 intro_slider 或自訂）
- 追蹤使用者是否已完成引導
- 實作「跳過」和「重新開啟」功能

---

## Epic 3: 商品管理核心功能

**目標：** 讓使用者可以輕鬆發布、瀏覽、搜尋和管理商品，建立完整的商品生態系統。

### Story 3.1: 商品發布功能（簡化版）

As a **已驗證使用者**,
I want **快速發布商品到我的社區**,
So that **鄰居可以看到並購買我的商品**。

**Acceptance Criteria:**

**Given** 使用者已完成社區驗證
**When** 點擊「發布商品」
**Then** 可以填寫：
- 商品照片（必須 4 張，支援拍照或相簿選擇）
- 商品狀態（全新/二手）
- 商品網址（選填，自動抓取標題）
- 商品標題（3-50字，必填）
- 商品摘要（20-200字，必填）
- 使用時間（選擇時間區間）
- 價格設定（固定價格/可議價/免費贈送）
- 發布社區（預設主要社區）

**And** 照片自動壓縮（單張 < 1MB）
**And** 提供商品網址時自動抓取標題
**And** 所有必填欄位完成才可發布
**And** 發布成功後自動跳轉到商品詳情頁
**And** 整個發布流程 < 2分鐘完成

**Prerequisites:** Story 2.5, Story 1.4

**Technical Notes:**
- 建立商品發布表單 UI
- 整合圖片上傳功能（Story 1.4）
- 實作商品網址 metadata 抓取（Supabase Edge Function）
- 實作商品分類自動判斷
- 建立 items 和 item_images 資料表關聯

---

### Story 3.2: 商品列表與動態首頁

As a **使用者**,
I want **在首頁看到社區的最新商品動態**,
So that **我可以快速發現感興趣的商品**。

**Acceptance Criteria:**

**Given** 使用者已登入
**When** 開啟應用程式首頁
**Then** 顯示：
- 主要社區的最新商品（依發布時間倒序）
- 商品卡顯示：主圖、標題、價格、發布時間、留言數、狀態標籤（全新/二手）
- 下拉重新整理功能
- 滾動載入更多（分頁載入）

**And** 已售出商品自動排到最後
**And** 無商品時顯示空狀態提示
**And** 頁面載入時間 < 2秒
**And** 點擊商品卡進入商品詳情

**Prerequisites:** Story 3.1

**Technical Notes:**
- 建立商品列表 UI（瀑布流或網格）
- 實作分頁載入機制
- 實作下拉重新整理
- 優化圖片載入（漸進式載入、快取）
- 實作社區切換功能

---

### Story 3.3: 商品詳情頁面

As a **使用者**,
I want **查看商品的完整資訊**,
So that **我可以了解商品詳情並決定是否購買**。

**Acceptance Criteria:**

**Given** 使用者點擊商品卡
**When** 進入商品詳情頁面
**Then** 顯示：
- 照片輪播（4張照片，支援左右滑動、點擊放大）
- 商品標題與價格（醒目顯示）
- 商品狀態標籤（全新/二手、上架中/已預訂/已售出）
- 使用時間（二手商品顯示）
- 商品摘要（完整顯示，支援換行與emoji）
- 商品資訊來源（若有提供，可點擊開啟外部瀏覽器）
- 商品分類標籤
- 發布時間（X分鐘前、X小時前、X天前）
- 賣家資訊區塊（頭像、暱稱、認證徽章、信用評分、交易次數、所屬社區）
- 留言區（顯示所有留言）

**And** 點擊賣家資訊可查看賣家主頁
**And** 已售出商品顯示「此商品已售出」狀態且留言輸入框禁用

**Prerequisites:** Story 3.1, Story 2.6

**Technical Notes:**
- 建立商品詳情頁 UI
- 實作照片輪播和放大功能
- 整合商品網址開啟功能（url_launcher）
- 實作賣家資訊區塊
- 整合留言區（見 Epic 4）

---

### Story 3.4: 商品管理功能

As a **商品賣家**,
I want **管理我發布的商品**,
So that **我可以更新商品資訊、管理狀態和處理交易**。

**Acceptance Criteria:**

**Given** 使用者已發布商品
**When** 進入「我的商品」列表
**Then** 可以：
- 查看商品列表（分類：上架中、已預訂、已售出、已下架）
- 編輯商品（修改標題、價格、描述、照片）
- 標記為已預訂（暫時保留給特定買家）
- 標記為已售出（選擇成交買家，觸發交易確認流程）
- 下架商品
- 重新上架（將已下架或已預訂商品重新發布）
- 刪除商品（永久刪除，需二次確認）

**And** 編輯後即時更新商品詳情
**And** 狀態變更後留言區會顯示對應提示

**Prerequisites:** Story 3.1

**Technical Notes:**
- 建立「我的商品」列表頁面
- 實作商品狀態管理邏輯
- 實作商品編輯功能
- 實作刪除確認機制
- 整合交易確認流程（見 Epic 5）

---

### Story 3.5: 分類瀏覽功能

As a **使用者**,
I want **依照商品分類瀏覽**,
So that **我可以快速找到特定類別的商品**。

**Acceptance Criteria:**

**Given** 使用者在首頁
**When** 點擊分類標籤（家具家飾、家電用品、3C產品等）
**Then** 篩選顯示該類別的商品
**And** 保持在當前社區範圍內
**And** 分類計數顯示（每個分類下的商品數量）
**And** 選中分類高亮顯示
**And** 無商品分類顯示「此分類暫無商品」

**Prerequisites:** Story 3.2

**Technical Notes:**
- 建立分類導航列（橫向滾動）
- 實作分類篩選邏輯
- 實作分類計數查詢
- 優化篩選效能

---

### Story 3.6: 關鍵字搜尋功能

As a **使用者**,
I want **透過關鍵字搜尋商品**,
So that **我可以快速找到想要的商品**。

**Acceptance Criteria:**

**Given** 使用者在首頁
**When** 在搜尋框輸入關鍵字（2個字元以上）
**Then** 即時顯示搜尋結果（搜尋範圍：商品標題、描述）
**And** 搜尋結果依相關度排序
**And** 顯示搜尋歷史紀錄（最近5筆）
**And** 顯示熱門搜尋關鍵字
**And** 無結果時顯示「找不到相關商品」提示
**And** 可清除搜尋歷史
**And** 搜尋結果顯示時間 < 1秒

**Prerequisites:** Story 3.2

**Technical Notes:**
- 建立搜尋 UI（搜尋框、結果列表）
- 實作即時搜尋功能（使用 Supabase 全文搜尋或 PostgreSQL tsvector）
- 實作搜尋歷史儲存（本地儲存）
- 實作熱門搜尋統計

---

## Epic 4: 溝通與互動系統

**目標：** 讓買賣雙方可以透過留言板進行溝通，並透過推播通知即時接收訊息。

### Story 4.1: 商品留言板基礎功能

As a **已驗證使用者**,
I want **在商品頁面發表留言**,
So that **我可以詢問商品詳情或表達購買意願**。

**Acceptance Criteria:**

**Given** 使用者已完成社區驗證
**When** 在商品詳情頁面點擊「留言」
**Then** 可以：
- 輸入文字留言（最多200字）
- 選擇「公開」或「僅賣家可見」（隱私留言）
- 使用快速留言範本（6種常用語句）
- 支援emoji輸入

**And** 留言即時顯示在留言區（< 2秒）
**And** 公開留言所有人可見，隱私留言僅賣家與留言者可見
**And** 已售出商品無法留言

**Prerequisites:** Story 3.3, Story 2.5

**Technical Notes:**
- 建立留言輸入 UI
- 實作公開/隱私留言切換
- 建立 comments 資料表
- 實作留言即時更新（Supabase Realtime）
- 實作留言權限控制（RLS）

---

### Story 4.2: 留言顯示與管理

As a **使用者**,
I want **查看和管理商品留言**,
So that **我可以了解其他使用者的詢問和賣家的回覆**。

**Acceptance Criteria:**

**Given** 使用者在商品詳情頁面
**When** 查看留言區
**Then** 顯示：
- 所有留言（按時間倒序，最新在上）
- 公開留言：完整顯示留言者頭像、暱稱、認證徽章、留言內容、時間
- 隱私留言：對其他人顯示「🔒 私密留言」，僅賣家與留言者可見完整內容
- 賣家回覆（以縮排或特殊背景色顯示，標記「賣家回覆」）
- 留言計數顯示

**And** 賣家可以刪除不當留言
**And** 留言可以複製文字

**Prerequisites:** Story 4.1

**Technical Notes:**
- 建立留言顯示 UI
- 實作留言排序邏輯
- 實作隱私留言權限控制
- 實作賣家留言管理功能

---

### Story 4.3: 一鍵議價功能

As a **買家**,
I want **使用一鍵議價功能**,
So that **我可以快速提出議價並得到賣家回應**。

**Acceptance Criteria:**

**Given** 商品支援議價
**When** 點擊「💰 議價」按鈕
**Then** 彈出議價介面：
- 顯示商品原價
- 滑桿或數字輸入框設定期望價格
- 自動計算折扣百分比（例：「原價8折」）
- 可選擇「立即議價」或「先詢問」

**And** 送出後自動產生議價留言（預設隱私）
**And** 賣家收到議價通知（特殊標記 💰 議價留言）
**And** 賣家可一鍵回覆：「接受」「拒絕」「再議」

**Prerequisites:** Story 4.1

**Technical Notes:**
- 建立議價 UI（滑桿、數字輸入）
- 實作議價留言自動生成
- 實作賣家快速回覆功能
- 整合通知系統（見 Story 4.4）

---

### Story 4.4: 推播通知系統

As a **使用者**,
I want **收到留言和交易的推播通知**,
So that **我不會錯過重要的訊息**。

**Acceptance Criteria:**

**Given** 使用者已啟用推播通知
**When** 發生以下事件：
- 我的商品收到新留言
- 我的留言被賣家回覆
- 交易狀態更新

**Then** 收到推播通知：
- 通知標題和內容清晰
- 點擊通知跳轉到對應頁面
- 通知延遲 < 5秒

**And** 使用者可在設定中關閉特定類型通知
**And** 支援夜間模式（23:00-08:00）靜音
**And** 通知分組（同一商品的多個留言合併顯示）

**Prerequisites:** Story 1.5, Story 4.1

**Technical Notes:**
- 實作 Supabase Database Trigger 監聽留言事件
- 實作 Supabase Edge Function 發送推播
- 整合 FCM API
- 實作通知設定管理
- 實作 Deep Link 跳轉

---

### Story 4.5: 留言通知中心

As a **使用者**,
I want **在通知中心查看所有留言通知**,
So that **我可以集中管理所有訊息**。

**Acceptance Criteria:**

**Given** 使用者收到留言通知
**When** 進入通知中心
**Then** 顯示：
- 通知列表（依時間倒序）
- 通知類型圖示
- 留言者頭像、暱稱
- 留言內容摘要（前30字）
- 商品縮圖與標題
- 時間
- 未讀標記（紅點）

**And** 點擊通知跳轉到對應商品頁面的留言位置
**And** 點擊通知後標記為已讀
**And** 通知保存30天
**And** 可批次清除已讀通知

**Prerequisites:** Story 4.4

**Technical Notes:**
- 建立通知中心 UI
- 實作通知列表查詢
- 實作通知已讀/未讀狀態管理
- 實作 Deep Link 跳轉到留言位置

---

## Epic 5: 交易流程

**目標：** 提供完整的交易流程，包括交易確認、彈性交易模式（管理室代收、大廳面交）和錯誤處理機制。

### Story 5.1: 交易完成確認流程

As a **賣家**,
I want **標記商品為已售出並選擇成交買家**,
So that **系統可以記錄交易並觸發評價流程**。

**Acceptance Criteria:**

**Given** 賣家已發布商品並有留言者
**When** 在商品管理頁面點擊「標記為已售出」
**Then** 系統顯示該商品的所有留言者列表
**And** 賣家選擇實際成交的買家（從留言者中選擇）
**And** 確認後商品狀態變更為「已售出」
**And** 買家收到通知「您是否完成了【商品名稱】的交易？」
**And** 買家確認後觸發雙方評價流程
**And** 雙方交易次數+1
**And** 交易紀錄保存

**Prerequisites:** Story 3.4, Story 4.1

**Technical Notes:**
- 建立交易確認 UI
- 實作買家選擇邏輯
- 建立 transactions 資料表
- 實作交易狀態管理
- 整合評價流程（見 Epic 6）

---

### Story 5.2: 彈性交易模式選擇

As a **賣家**,
I want **選擇交易方式（管理室代收或大廳面交）**,
So that **買賣雙方可以選擇最便利的交易方式**。

**Acceptance Criteria:**

**Given** 賣家標記商品為已售出
**When** 選擇交易方式
**Then** 可以選擇：
- 管理室代收（系統產生6位數代收編號）
- 社區公共區域面交（大廳、中庭、郵件室）
- 門口/樓下面交（約定地點）

**And** 系統記錄交易地點
**And** 選擇管理室代收時通知管理員
**And** 買家收到交易方式通知

**Prerequisites:** Story 5.1

**Technical Notes:**
- 建立交易方式選擇 UI
- 實作代收編號生成邏輯（6位數字，唯一）
- 實作交易地點記錄
- 整合管理室後台通知（見 Epic 8）

---

### Story 5.3: 管理室代收流程 - 賣家端

As a **賣家**,
I want **將商品送至管理室代收**,
So that **買家可以在方便的時間取貨**。

**Acceptance Criteria:**

**Given** 賣家選擇「管理室代收」
**When** 將商品送至管理室
**Then** 系統產生「代收編號」（6位數字）
**And** 賣家告知管理員代收編號
**And** 管理員在後台登記「已收到商品」
**And** 買家收到通知「商品已送達管理室，請攜帶編號前往取貨」

**Prerequisites:** Story 5.2, Story 8.2（管理室後台代收管理）

**Technical Notes:**
- 實作代收編號生成和顯示
- 整合管理室後台 API
- 實作代收狀態追蹤

---

### Story 5.4: 管理室代收流程 - 買家端

As a **買家**,
I want **從管理室取貨並付款**,
So that **我可以完成交易**。

**Acceptance Criteria:**

**Given** 買家收到「商品已送達管理室」通知
**When** 前往管理室取貨
**Then** 告知管理員代收編號
**And** 管理員核對編號，交付商品
**And** 買家現場驗貨
**And** 買家現場付款（現金交給管理員）
**And** 管理員在後台確認「已取貨」
**And** 系統通知雙方「交易完成」，觸發評價流程

**Prerequisites:** Story 5.3

**Technical Notes:**
- 實作代收編號顯示（買家端）
- 整合管理室後台確認流程
- 實作交易完成觸發邏輯

---

### Story 5.5: 大廳面交流程

As a **買賣雙方**,
I want **在社區大廳完成面交**,
So that **我們可以在公共安全區域完成交易**。

**Acceptance Criteria:**

**Given** 賣家選擇「大廳面交」
**When** 雙方約定時間和地點
**Then** 系統提供面交地點建議（大廳、中庭、郵件室）
**And** 地點標記安全資訊（有監視器、管理室可見範圍、建議白天交易）
**And** 雙方在 App 中確認「已完成現金交易」
**And** 系統記錄交易完成時間
**And** 觸發評價流程

**Prerequisites:** Story 5.2

**Technical Notes:**
- 建立面交地點選擇 UI
- 實作面交確認機制
- 實作安全提示顯示

---

### Story 5.6: 交易失敗處理流程

As a **使用者**,
I want **在交易失敗時有明確的處理流程**,
So that **我可以從錯誤中恢復並繼續使用平台**。

**Acceptance Criteria:**

**Given** 交易過程中發生異常
**When** 發生以下情況：
- 賣家取消預訂
- 買家取消預訂
- 商品未送達管理室（超過24小時）
- 買家未取貨（超過7天）

**Then** 系統提供對應的處理流程：
- 自動通知相關方
- 商品狀態自動恢復
- 記錄取消原因（選填）
- 顯示友善的錯誤訊息

**Prerequisites:** Story 5.1, Story 5.3

**Technical Notes:**
- 實作交易取消邏輯
- 實作自動提醒機制（定時任務）
- 實作狀態自動恢復
- 建立錯誤處理機制

---

### Story 5.7: 支付糾紛處理機制

As a **使用者**,
I want **在支付出現糾紛時有處理機制**,
So that **我可以解決支付問題**。

**Acceptance Criteria:**

**Given** 交易完成後
**When** 發生支付糾紛：
- 金額不符
- 未收到款項

**Then** 可以提出爭議：
- 選擇爭議類型
- 填寫爭議說明
- 系統記錄雙方聲稱的金額
- 提供平台仲裁機制（見 Story 7.3）

**Prerequisites:** Story 5.4, Story 5.5

**Technical Notes:**
- 建立支付糾紛申訴 UI
- 實作爭議記錄功能
- 整合平台仲裁流程

---

## Epic 6: 信任與評價系統

**目標：** 建立信任機制，讓使用者可以透過評價和交易歷史建立信譽。

### Story 6.1: 評價系統

As a **交易完成的使用者**,
I want **對交易對象進行評價**,
So that **我可以分享交易體驗並幫助建立社區信任**。

**Acceptance Criteria:**

**Given** 交易完成後
**When** 系統彈出評價提示
**Then** 可以：
- 選擇星等評分（1-5星）
- 填寫文字評價（選填，最多100字）
- 送出評價

**And** 評價匿名性（對方不知道誰先評價）
**And** 雙方評價後才顯示彼此評價內容
**And** 評價無法修改（送出後鎖定）
**And** 評價成功後更新信用分數
**And** 可跳過評價（7天後自動關閉評價視窗）

**Prerequisites:** Story 5.1

**Technical Notes:**
- 建立評價 UI
- 建立 reviews 資料表
- 實作評價匿名機制
- 實作信用分數計算邏輯
- 實作評價顯示規則

---

### Story 6.2: 個人主頁

As a **使用者**,
I want **查看自己和其他使用者的個人主頁**,
So that **我可以了解對方的信譽和交易歷史**。

**Acceptance Criteria:**

**Given** 使用者在商品詳情頁面
**When** 點擊賣家資訊
**Then** 進入個人主頁，顯示：
- 個人資訊：頭像、暱稱、認證徽章、信用評分（星級顯示）、交易次數（買入X次、賣出X次）、加入時間、所屬社區
- 正在出售的商品列表
- 收到的評價列表（顯示最新5筆）

**And** 點擊商品可進入詳情頁
**And** 自己的主頁可編輯資料

**Prerequisites:** Story 2.2, Story 3.1, Story 6.1

**Technical Notes:**
- 建立個人主頁 UI
- 實作個人資料顯示
- 實作商品列表查詢
- 實作評價列表顯示

---

### Story 6.3: 交易紀錄查詢

As a **使用者**,
I want **查詢我的歷史交易紀錄**,
So that **我可以追蹤過去的交易和評價狀態**。

**Acceptance Criteria:**

**Given** 使用者已登入
**When** 進入「交易紀錄」頁面
**Then** 顯示交易列表分類：
- 我買到的
- 我賣出的

**And** 每筆交易顯示：
- 商品圖片與標題
- 交易對象
- 交易日期
- 交易金額
- 評價狀態（已評價/未評價）

**And** 點擊交易可查看詳情
**And** 未評價的交易可補評價（7天內）
**And** 依時間倒序排列
**And** 可搜尋交易紀錄

**Prerequisites:** Story 5.1, Story 6.1

**Technical Notes:**
- 建立交易紀錄列表 UI
- 實作交易紀錄查詢
- 實作交易分類篩選
- 實作交易搜尋功能

---

## Epic 7: 安全與內容管理

**目標：** 確保平台安全，提供檢舉、封鎖和爭議處理機制，維護良好的交易環境。

### Story 7.1: 檢舉機制

As a **使用者**,
I want **檢舉不當商品或不良使用者**,
So that **我可以維護平台的安全和品質**。

**Acceptance Criteria:**

**Given** 使用者在商品詳情頁面或使用者主頁
**When** 點擊「檢舉」按鈕
**Then** 可以選擇檢舉原因：
- 販售違禁品
- 詐騙行為
- 不實商品描述
- 騷擾或不當言論
- 其他（需填寫說明）

**And** 可附上截圖證據（選填）
**And** 檢舉後可選擇封鎖該使用者
**And** 檢舉成功後顯示確認訊息
**And** 檢舉提交到後台審核

**Prerequisites:** Story 3.3, Story 6.2

**Technical Notes:**
- 建立檢舉 UI
- 建立 reports 資料表
- 實作檢舉提交邏輯
- 整合後台審核流程

---

### Story 7.2: 封鎖功能

As a **使用者**,
I want **封鎖不想互動的對象**,
So that **我可以避免與特定使用者互動**。

**Acceptance Criteria:**

**Given** 使用者在個人主頁
**When** 點擊「封鎖」按鈕
**Then** 顯示二次確認
**And** 確認後：
- 對方無法查看我的商品
- 對方無法聯絡我
- 我不會看到對方的商品

**And** 封鎖名單可在設定頁面管理
**And** 可解除封鎖

**Prerequisites:** Story 6.2

**Technical Notes:**
- 建立封鎖功能邏輯
- 建立 blocked_users 資料表
- 實作封鎖篩選邏輯（商品列表、留言等）
- 實作封鎖名單管理 UI

---

### Story 7.3: 商品描述不符的爭議處理

As a **買家**,
I want **在發現商品與描述不符時提出爭議**,
So that **我可以獲得公平的處理**。

**Acceptance Criteria:**

**Given** 交易完成後7天內
**When** 買家提出爭議
**Then** 可以：
- 選擇爭議類型（商品描述不符、商品損壞或缺失、商品功能異常、其他）
- 上傳證據照片（最多5張）
- 填寫爭議說明（必填，50-500字）

**And** 賣家收到「交易爭議通知」
**And** 賣家可選擇：接受爭議、提出反駁（需提供證據）、請求平台仲裁
**And** 當雙方無法達成共識時，可請求平台仲裁
**And** 平台管理員查看爭議內容和證據並做出裁決
**And** 裁決結果通知雙方
**And** 爭議記錄影響賣家信用評分

**Prerequisites:** Story 5.1, Story 6.1

**Technical Notes:**
- 建立爭議申訴 UI
- 建立 disputes 資料表
- 實作證據上傳功能
- 實作平台仲裁流程
- 整合信用評分影響邏輯

---

## Epic 8: 管理室後台系統

**目標：** 為管理員提供完整的 Web 後台系統，管理社區、住戶驗證、代收商品和數據統計。

### Story 8.1: 管理員後台基礎架構

As a **管理員**,
I want **登入 Web 後台系統**,
So that **我可以管理社區和住戶**。

**Acceptance Criteria:**

**Given** 管理員有後台帳號
**When** 訪問後台網址
**Then** 可以：
- 使用 Email + 密碼登入
- 登入後顯示管理的社區清單
- 支援管理多個社區（同一管理員可管理多個社區）

**And** 登入狀態持久保存
**And** 登出功能正常

**Prerequisites:** Story 1.2

**Technical Notes:**
- 建立 React/Vue.js Web 後台專案
- 實作管理員認證系統（Supabase Auth）
- 建立後台路由和導航
- 實作響應式設計（支援平板、桌機）

---

### Story 8.2: 管理室代收商品管理

As a **管理員**,
I want **在後台管理代收商品**,
So that **我可以協助完成交易流程**。

**Acceptance Criteria:**

**Given** 管理員已登入後台
**When** 進入代收商品管理介面
**Then** 顯示待處理代收清單：
- 代收編號
- 賣家、買家資訊
- 商品名稱
- 金額
- 狀態（待送達、已收到、已取貨、已完成）

**And** 可以：
- 登記「已收到商品」（掃描或輸入代收編號，確認商品與賣家身份，拍照留存選填）
- 登記「已取貨」（確認買家身份，商品交給買家，收取現金，標記「已取貨」）
- 管理現金（記錄收到的現金金額，待賣家領取現金清單，現金交付記錄）

**Prerequisites:** Story 8.1, Story 5.2

**Technical Notes:**
- 建立代收商品管理 UI
- 實作代收狀態管理
- 實作現金管理功能
- 整合交易狀態更新 API

---

### Story 8.3: 管理員權限管理

As a **超級管理員**,
I want **管理不同層級的管理員權限**,
So that **不同角色可以執行對應的操作**。

**Acceptance Criteria:**

**Given** 超級管理員已登入
**When** 進入權限管理介面
**Then** 可以：
- 建立/管理管理員帳號
- 設定權限層級：
  - 超級管理員：建立/管理所有社區、建立/管理所有管理員帳號、查看所有社區數據、系統設定、爭議仲裁
  - 社區管理員：管理指定社區的住戶驗證、產生 QR Code、查看該社區數據、管理該社區代收商品
  - 管理室操作員：僅能操作代收商品管理、登記「已收到商品」、登記「已取貨」、現金管理

**And** 使用 Supabase Row Level Security (RLS) 實現權限控制
**And** 每個操作都記錄操作者和時間

**Prerequisites:** Story 8.1

**Technical Notes:**
- 建立權限管理 UI
- 實作權限層級邏輯
- 設定 RLS 政策
- 整合操作日誌（見 Story 8.4）

---

### Story 8.4: 操作日誌記錄

As a **管理員**,
I want **查看操作日誌**,
So that **我可以追蹤所有管理操作**。

**Acceptance Criteria:**

**Given** 管理員已登入後台
**When** 進入操作日誌頁面
**Then** 顯示操作記錄：
- 操作時間
- 操作者（管理員帳號）
- 操作類型
- 操作對象
- IP 位址
- 操作結果（成功/失敗）

**And** 管理員可查看自己的操作記錄
**And** 超級管理員可查看所有操作記錄
**And** 支援依時間、操作類型、操作者篩選
**And** 日誌保留90天

**Prerequisites:** Story 8.1

**Technical Notes:**
- 建立操作日誌記錄機制（Database Trigger）
- 建立 admin_logs 資料表
- 建立操作日誌查詢 UI
- 實作日誌篩選功能

---

### Story 8.5: 批量操作功能

As a **管理員**,
I want **批量處理操作**,
So that **我可以提高工作效率**。

**Acceptance Criteria:**

**Given** 管理員已登入後台
**When** 執行批量操作
**Then** 可以：
- 批量產生 QR Code（選擇多個門牌號碼，一次產生多個 QR Code）
- 批量匯入門牌資料（上傳 CSV 檔案，自動解析並建立門牌資料）
- 批量標記驗證狀態（選擇多個住戶，批量標記為「已驗證」或「未驗證」）

**And** 錯誤檢查和修正機制正常
**And** 操作結果正確

**Prerequisites:** Story 8.1, Story 2.3

**Technical Notes:**
- 建立批量操作 UI
- 實作 CSV 匯入功能
- 實作批量 QR Code 產生
- 實作錯誤檢查和修正

---

### Story 8.6: 異常交易警示機制

As a **管理員**,
I want **收到異常交易警示**,
So that **我可以及時發現和處理異常情況**。

**Acceptance Criteria:**

**Given** 系統檢測到異常交易
**When** 發生以下情況：
- 短時間大量交易（同一使用者24小時內完成10筆以上）
- 高金額交易（單筆交易金額超過 NT$ 10,000）
- 爭議交易（同一賣家多次被提出爭議）
- 異常 IP（同一帳號從多個不同 IP 登入）

**Then** 系統自動發送警示通知給管理員
**And** 管理員可查看異常交易詳情
**And** 管理員可標記為「已處理」或「需進一步調查」

**Prerequisites:** Story 8.1, Story 5.1

**Technical Notes:**
- 實作異常交易檢測邏輯（Database Function 或 Edge Function）
- 建立異常交易警示 UI
- 實作警示通知機制
- 實作異常交易處理流程

---

### Story 8.7: 數據統計面板

As a **管理員**,
I want **查看社區數據統計**,
So that **我可以了解社區的使用情況**。

**Acceptance Criteria:**

**Given** 管理員已登入後台
**When** 進入數據統計面板
**Then** 顯示：
- 社區總住戶數
- 已驗證住戶數 / 驗證率
- 本週新增驗證數量
- 活躍使用者數（近7天有登入）
- 社區商品數量
- 社區交易次數
- 圖表顯示（驗證趨勢、活躍度）

**And** 數據即時更新

**Prerequisites:** Story 8.1

**Technical Notes:**
- 建立數據統計 Dashboard UI
- 實作數據查詢和聚合邏輯
- 整合圖表庫（Chart.js 或類似）
- 實作數據快取機制

---

## Epic 9: 數據隱私與合規

**目標：** 確保平台符合台灣個人資料保護法要求，保護使用者隱私。

### Story 9.1: 門牌號碼顯示規則

As a **使用者**,
I want **我的門牌號碼受到隱私保護**,
So that **我的個人資訊不會過度曝光**。

**Acceptance Criteria:**

**Given** 使用者已完成社區驗證
**When** 其他使用者查看我的資訊
**Then** 僅顯示「棟別」和「樓層」（例：「A棟 3樓」）
**And** 不顯示完整門牌號碼（例：不顯示「301號」）
**And** 我自己可以看到完整門牌號碼
**And** 管理員可查看完整門牌號碼
**And** 可在設定中選擇是否顯示完整地址（預設：僅顯示棟別和樓層）

**Prerequisites:** Story 2.5

**Technical Notes:**
- 實作門牌號碼顯示邏輯
- 建立隱私設定功能
- 在相關 UI 位置應用顯示規則

---

### Story 9.2: 個人資料保護措施

As a **使用者**,
I want **我的個人資料受到安全保護**,
So that **我的隱私不會被濫用**。

**Acceptance Criteria:**

**Given** 使用者已註冊
**When** 系統收集和使用個人資料
**Then** 僅收集必要的個人資料（暱稱、Email、頭像、社區資訊）
**And** 不收集：真實姓名、手機號碼、身份證字號、銀行帳號
**And** 所有敏感資料加密儲存
**And** 使用 Supabase Row Level Security (RLS) 確保資料存取權限
**And** 個人資料僅用於平台功能，不向第三方分享

**Prerequisites:** Story 2.1, Story 1.3

**Technical Notes:**
- 實作資料收集最小化原則
- 設定資料加密儲存
- 強化 RLS 政策
- 建立資料使用限制機制

---

### Story 9.3: GDPR/個資法合規性功能

As a **使用者**,
I want **行使我的個資法權利**,
So that **我可以控制我的個人資料**。

**Acceptance Criteria:**

**Given** 使用者已登入
**When** 進入隱私設定頁面
**Then** 可以：
- 查看我的所有個人資料（資料查詢權）
- 下載我的資料（JSON 格式，資料可攜權）
- 更正我的個人資料（資料更正權）
- 申請刪除帳號和所有個人資料（資料刪除權，刪除後30天內可恢復）

**And** 首次使用時顯示隱私政策
**And** 使用者需同意隱私政策才能使用平台
**And** 隱私政策更新時通知使用者
**And** 資料保留政策正確執行（交易記錄2年、評價記錄2年、商品資料1年）

**Prerequisites:** Story 2.2

**Technical Notes:**
- 建立隱私設定 UI
- 實作資料下載功能（JSON 格式）
- 實作帳號刪除功能（軟刪除，30天恢復期）
- 建立隱私政策文件
- 實作資料保留政策自動清理機制

---

## Epic 10: 使用者體驗優化

**目標：** 優化使用者體驗，提供友善的錯誤處理、載入狀態和離線功能。

### Story 10.1: 錯誤訊息設計

As a **使用者**,
I want **看到友善、清晰的錯誤訊息**,
So that **我可以理解問題並知道如何解決**。

**Acceptance Criteria:**

**Given** 系統發生錯誤
**When** 顯示錯誤訊息
**Then** 錯誤訊息：
- 使用友善的語言（避免技術術語）
- 清楚說明問題和解決方法
- 使用圖示和顏色區分不同類型的錯誤
- 提供解決行動按鈕（例如：「重試」「重新掃描」「前往驗證」）

**And** 錯誤訊息類型包括：
- 網路錯誤：「網路連線不穩，請稍後再試」+「重試」按鈕
- 驗證錯誤：「QR Code 已過期，請重新掃描」+「重新掃描」按鈕
- 權限錯誤：「此功能需要完成社區驗證」+「前往驗證」按鈕
- 資料錯誤：「請檢查輸入的資料是否正確」+ 標示錯誤欄位

**Prerequisites:** Story 2.1, Story 2.5

**Technical Notes:**
- 建立錯誤訊息元件庫
- 實作錯誤訊息分類和顯示邏輯
- 整合錯誤處理機制

---

### Story 10.2: 載入狀態處理

As a **使用者**,
I want **看到清楚的載入狀態**,
So that **我知道系統正在處理我的請求**。

**Acceptance Criteria:**

**Given** 系統正在處理操作
**When** 發生以下情況：
- 頁面載入
- 資料上傳
- 操作處理

**Then** 顯示對應的載入狀態：
- 頁面載入：顯示載入動畫（Skeleton Screen），顯示「載入中...」文字
- 資料上傳：顯示上傳進度條，顯示「上傳中... X%」文字
- 操作處理：顯示處理動畫，顯示「處理中...」文字

**And** 使用漸進式載入（先顯示基本內容，再載入詳細內容）
**And** 使用快取減少載入時間
**And** 預載入下一頁內容

**Prerequisites:** Story 1.2

**Technical Notes:**
- 建立載入狀態元件（Skeleton Screen、Progress Bar）
- 實作漸進式載入機制
- 實作快取策略
- 實作預載入機制

---

### Story 10.3: 離線功能支援

As a **使用者**,
I want **在網路不穩定時仍能使用基本功能**,
So that **我不會因為網路問題而無法使用應用程式**。

**Acceptance Criteria:**

**Given** 網路連線不穩定或中斷
**When** 使用應用程式
**Then** 可以離線使用：
- 瀏覽已載入的商品列表
- 查看已載入的商品詳情
- 查看已載入的留言
- 查看個人資料和交易記錄
- 編輯草稿（未發布的商品）

**And** 不可離線使用：
- 發布新商品
- 發表新留言
- 完成交易確認
- 搜尋新商品

**And** 顯示「離線模式」提示
**And** 顯示「正在同步資料」狀態
**And** 網路恢復時自動同步

**Prerequisites:** Story 1.2, Story 3.2

**Technical Notes:**
- 實作離線資料儲存（Brick/SQLite）
- 實作資料同步機制
- 實作離線模式檢測
- 實作自動同步邏輯

---

### Story 10.4: 系統錯誤處理與重試機制

As a **使用者**,
I want **系統在發生錯誤時自動重試**,
So that **我不需要手動處理暫時性的錯誤**。

**Acceptance Criteria:**

**Given** 系統發生暫時性錯誤（網路中斷、API 失敗）
**When** 執行操作失敗
**Then** 系統自動重試（最多3次）
**And** 顯示友善錯誤訊息
**And** 提供「手動重試」按鈕
**And** 系統錯誤不會導致資料遺失

**Prerequisites:** Story 10.1

**Technical Notes:**
- 實作自動重試機制
- 實作錯誤恢復邏輯
- 實作資料保護機制（防止資料遺失）

---

## Epic 總結

### Epic 執行順序建議

**Phase 1: 基礎建設（必須先完成）**
1. Epic 1: 專案基礎建設

**Phase 2: 核心功能（MVP 必須）**
2. Epic 2: 使用者認證與身份驗證
3. Epic 3: 商品管理核心功能
4. Epic 4: 溝通與互動系統
5. Epic 5: 交易流程

**Phase 3: 信任與安全（MVP 必須）**
6. Epic 6: 信任與評價系統
7. Epic 7: 安全與內容管理
8. Epic 8: 管理室後台系統

**Phase 4: 合規與優化（MVP 後期）**
9. Epic 9: 數據隱私與合規
10. Epic 10: 使用者體驗優化

### 總 Story 數量

- **Epic 1:** 5 個 Story
- **Epic 2:** 7 個 Story
- **Epic 3:** 6 個 Story
- **Epic 4:** 5 個 Story
- **Epic 5:** 7 個 Story
- **Epic 6:** 3 個 Story
- **Epic 7:** 3 個 Story
- **Epic 8:** 7 個 Story
- **Epic 9:** 3 個 Story
- **Epic 10:** 4 個 Story

**總計：50 個 Story**

### 依賴關係

- Epic 1 是基礎，所有其他 Epic 都依賴它
- Epic 2 的 Story 2.5（社區驗證）是許多功能的先決條件
- Epic 3 依賴 Epic 2（需要已驗證使用者）
- Epic 4 依賴 Epic 3（需要商品功能）
- Epic 5 依賴 Epic 3 和 Epic 4（需要商品和留言功能）
- Epic 6 依賴 Epic 5（需要交易功能）
- Epic 7 依賴 Epic 3 和 Epic 6（需要商品和個人主頁）
- Epic 8 可以與其他 Epic 並行開發（獨立後台系統）
- Epic 9 和 Epic 10 可以在開發過程中逐步整合

---

## 下一步行動

1. ✅ **Epic & Story 分解完成** - 50 個 Story 已定義
2. ⏭️ **UX 設計** - 執行 `workflow ux-design` 建立使用者流程與介面設計
3. ⏭️ **技術架構** - 執行 `workflow create-architecture` 定義技術決策與架構設計
4. ⏭️ **開始實作** - 從 Epic 1 開始，依序實作各 Story

---

_For implementation: Use the `create-story` workflow to generate individual story implementation plans from this epic breakdown._

