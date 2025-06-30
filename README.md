# vue-homework-day2 - 收藏網址小工具

這是一個使用 Vue.js Composition API 實作的收藏網址小工具，可以讓使用者收藏常用網址並進行管理。

## 使用技術

- Vue.js 3 (Composition API)
- Vite
- Tailwind CSS + DaisyUI

## 功能特色

### 🔖 新增收藏
- 輸入網址（必填）和標題（選填）
- 按下「新增收藏」按鈕或按 Enter 鍵即可加入收藏清單
- 網址為必填欄位，未填寫會顯示錯誤提示

### 📋 顯示清單
- 畫面下方顯示所有已收藏的網址
- 每個收藏項目可直接點擊開啟網址（新分頁）
- 顯示收藏總數量

### 🗑️ 刪除功能
- **刪除單一收藏**：每個收藏項目旁有刪除按鈕，點擊後確認即可移除
- **清空全部**：提供「清除全部」按鈕，可一次移除所有收藏

### 💾 本地儲存
- 使用 localStorage 自動儲存收藏資料
- 重新整理頁面或關閉瀏覽器後資料不會消失
- 開啟頁面時自動載入先前儲存的收藏

## 操作指南

### 新增收藏
1. 在「標題」欄位輸入網址的描述（選填）
2. 在「網址」欄位輸入要收藏的網址（必填）
3. 點擊「新增收藏」按鈕或按 Enter 鍵

### 開啟收藏的網址
- 直接點擊收藏清單中的任一項目即可在新分頁開啟

### 刪除收藏
- **刪除單一項目**：點擊收藏項目右側的 ❌ 按鈕，確認後刪除
- **清空全部**：點擊「清除全部」按鈕，確認後清空所有收藏

## IDE 推薦設定

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (請停用 Vetur 擴充功能)

## 自訂設定

參考 [Vite 設定文件](https://vite.dev/config/)

## 專案設定

```sh
npm install
```

### 開發環境編譯和熱重載

```sh
npm run dev
```

### 正式環境編譯和壓縮

```sh
npm run build
```

## 專案結構

```
src/
├── App.vue          # 主要應用程式元件
└── main.js         # 應用程式進入點
```

## 技術實作重點

- 使用 Vue 3 Composition API
- 響應式資料綁定 (ref)
- 生命週期鉤子 (onMounted)
- 本地儲存整合 (localStorage)
- 表單驗證和錯誤處理
- 現代化 UI 設計 (Tailwind CSS + DaisyUI)
